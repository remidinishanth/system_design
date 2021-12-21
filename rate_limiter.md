REF: https://towardsdatascience.com/designing-a-rate-limiter-6351bd8762c6

## Request rate limiter

* This rate limiter restricts each user to N requests per second. 
* Request rate limiters are the first tool most APIs can use to effectively manage a high volume of traffic.
* Others are inflight/concurrent request rate limit, location or ip based rate limiting


### Functional Requirements:
* `allowRequest(request) --> bool`
* A client can send a limited number of requests to a server within a time window, e.g., 10 requests per second.
* The client should get an error message(429, Too many requests) if the defined threshold limit of request is crossed for a single server or across different combinations of servers.

### Non-Functional Requirements:
* The system should be `highly available` since it protects our service from external attacks.
* `Low latency` - Performance is an important factor for any system. So, we need to be careful that rate limiter service should not add substantial latencies to the system.
* As `accurate` as possible.

### Storage
* UserID, Count, StartTime/StartMinute

Rate-limiter responsibility is to decide whether the client request will be served or declined. Middleware

![image](https://user-images.githubusercontent.com/19663316/146977816-f902f3ab-ad32-414c-baeb-33049ba301bf.png)

#### Fixed Minute algorithm
* Store the number of requests for the user, in the current minute
* Drawback: If there is a burst of requests at `12:01:59` and `12:02:01`, then we are allowing twice the limit of requests in a minute

#### Sliding Window algorithm
* Keep all the request in a queue, to check exactly -> requires lot of memory

#### Leaky Bucket Algorithm
* All the requests of a users are queued into a bucket, requests are processed at a fixed rate.
* Leaky bucket (closely related to token bucket) is an algorithm that provides a simple, intuitive approach to rate limiting via a queue, which you can think of as a bucket holding the requests. When registering a request, the system appends it to the end of the queue. Processing for the first item on the queue occurs at a regular interval or first in, first out (FIFO). If the queue is full, then additional requests are discarded (or leaked).

### Problems in Distributed Environment
If there is more than one rate limiter, then if both the instances read the database then they might allow some extra requests

![image](https://user-images.githubusercontent.com/19663316/146981596-89935546-210e-4711-bb01-8abbb5b482c2.png)

* To solve this, we might need to use `sticky session` load balancing to ensure that one user's request will only go to same rate limiter service. But then it's not fault tolerant design.
* If we use database `locks`, then there is some more latency.

We can also use cache to store the recent user's limit, this should be faster than going to the DB each time. We need to use `write-back`(writing to the db while evicting the entry/at periodic interval of fixed time) cache strategy to update the database.
