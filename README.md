## Resources

* https://github.com/binhnguyennus/awesome-scalability
* https://github.com/codersguild/System-Design
* https://github.com/donnemartin/system-design-primer
* https://systemdesign.org/
* https://completedesigninterviewcourse.com/system-design-concepts-components/
* https://github.com/Jeevan-kumar-Raj/Grokking-System-Design


## system_design interview

```
(1) FEATURE EXPECTATIONS [5 min]
        (1) Use cases
        (2) Scenarios that will not be covered
        (3) Who will use
        (4) How many will use
        (5) Usage patterns

(2) ESTIMATIONS [5 min]
        (1) Throughput (QPS for read and write queries)
        (2) Latency expected from the system (for read and write queries)
        (3) Read/Write ratio
        (4) Traffic estimates
                - Write (QPS, Volume of data)
                - Read  (QPS, Volume of data)
        (5) Storage estimates
        (6) Memory estimates
                - If we are using a cache, what is the kind of data we want to store in cache
                - How much RAM and how many machines do we need for us to achieve this ?
                - Amount of data you want to store in disk/ssd

(3) DESIGN GOALS [5 min]
        (1) Latency and Throughput requirements
        (2) Consistency vs Availability  [Weak/strong/eventual => consistency | Failover/replication => availability]

(4) HIGH LEVEL DESIGN [5-10 min]
        (1) APIs for Read/Write scenarios for crucial components
        (2) Database schema
        (3) Basic algorithm
        (4) High level design for Read/Write scenario

(5) DEEP DIVE [15-20 min]
        (1) Scaling the algorithm
        (2) Scaling individual components: 
                -> Availability, Consistency and Scale story for each component
                -> Consistency and availability patterns
        (3) Think about the following components, how they would fit in and how it would help
                a) DNS
                b) CDN [Push vs Pull]
                c) Load Balancers [Active-Passive, Active-Active, Layer 4, Layer 7]
                d) Reverse Proxy
                e) Application layer scaling [Microservices, Service Discovery]
                f) DB [RDBMS, NoSQL]
                        > RDBMS 
                            >> Master-slave, Master-master, Federation, Sharding, Denormalization, SQL Tuning
                        > NoSQL
                            >> Key-Value, Wide-Column, Graph, Document
                                Fast-lookups:
                                -------------
                                    >>> RAM  [Bounded size] => Redis, Memcached
                                    >>> AP [Unbounded size] => Cassandra, RIAK, Voldemort
                                    >>> CP [Unbounded size] => HBase, MongoDB, Couchbase, DynamoDB
                g) Caches
                        > Client caching, CDN caching, Webserver caching, Database caching, Application caching, Cache @Query level, Cache @Object level
                        > Eviction policies:
                                >> Cache aside
                                >> Write through
                                >> Write behind
                                >> Refresh ahead
                h) Asynchronism
                        > Message queues
                        > Task queues
                        > Back pressure
                i) Communication
                        > TCP
                        > UDP
                        > REST
                        > RPC

(6) JUSTIFY [5 min]
	(1) Throughput of each layer
	(2) Latency caused between each layer
	(3) Overall latency justification
```  
  
REF: https://leetcode.com/discuss/career/229177/my-system-design-template

SQL vs NOSQL: https://aaronice.gitbook.io/system-design/distributed-systems/sql-vs-nosql

Websockets vs Polling: https://aaronice.gitbook.io/system-design/distributed-systems/long-polling-vs-websockets-vs-server-sent-events


### 5 Tips for System Design interviews
-- by Gaurav Sen
1) Don't get into details prematurely
2) Avoid fitting requirements to a set architecture in mind
3) Keep it simple, stupid! Remember to look at the big picture and avoid too many hacks when solving.
4) Have justifications for the points you make. Don't use buzz words or half hearted thoughts in your design.
5) Be aware of the current solutions and tech practices. A lot of solutions can be purchased off the shelf which simplify implementation. You should be able to argue for a custom implementation with it's pros and cons.


### Here are three major points evaluated during the interview:
(1) Clarity of Thought
  * Express your thoughts in a clear manner.
  * Justify your decisions. Critical reasoning and argument are key to a successful software design.
  * When faced with a problem, use standard approaches to mitigate it. For example, say you are faced with an availability problem. State that replication and partitioning help increase availability in general, and move on to offer a solution. d) Don’t make points without thinking them through. Half-hearted attempts at solving problems are frowned upon heavily.
  
(2) Knowledge
  * Stay up to date with the current solutions in the market. This includes products and design practices. If NoSQL is being adopted left right and center, you need to be aware of it.
  * Know when to pick a solution vs. building something custom. If you name a product, you should be (generally) aware of the features it provides.
  * Design practices enable you to meet custom requirements. Examples are decoupling systems, load balancing, sticky sessions, etc…
  
(3) Flexibility
  * Switch your targets as the requirements shift. If the interviewer wants to know about one particular part of the system, do it first.
  * Never have a set architecture in mind. We all try to fit requirements to a system, but only after it has been shaped by the initial ones. A rigid attitude creates a brittle architecture. It will break before you do.
  * Take a step back at times to make adjustments to the general architecture. Being focused on one part can narrow our vision and bloat those areas. There will be components which can be extracted out and extended to the rest of the system.
