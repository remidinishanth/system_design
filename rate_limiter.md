## Request rate limiter

* This rate limiter restricts each user to N requests per second. 
* Request rate limiters are the first tool most APIs can use to effectively manage a high volume of traffic.


### Functional Requirements:
* A client can send a limited number of requests to a server within a time window, e.g., 10 requests per second.
* The client should get an error message(429, Too many requests) if the defined threshold limit of request is crossed for a single server or across different combinations of servers.

### Non-Functional Requirements:
* The system should be highly available since it protects our service from external attacks.
* Performance is an important factor for any system. So, we need to be careful that rate limiter service should not add substantial latencies to the system.

### Storage
* UserID, Count, StartTime

Rate-limiter responsibility is to decide whether the client request will be served or declined. Middleware
