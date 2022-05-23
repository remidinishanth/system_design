Read about Service Mesh https://www.nginx.com/blog/what-is-a-service-mesh/ and Envoy Proxy

## 𝐋𝐨𝐰 𝐋𝐞𝐯𝐞𝐥 𝐃𝐞𝐬𝐢𝐠𝐧 𝐐𝐮𝐞𝐬𝐭𝐢𝐨𝐧𝐬:
1. Design Parking Lot
    * Single Entry and Exit Gates
    * Multiple Entry and Exit Gates

2. Design Multiplayer Sudoku Game
    * Players have to Rotate the Pin in order to decide the turn.
    * Player who is able to fill the last empty cell will win.
3. Design LRU (Static + Dynamic Input Flow)
    * Follow Up : Implement LFU with Least Changes in the previous design (LRU)
4. Design In Memory Cache
5. Design Snake and Ladder Game
    * How would you change the design if wild cards are allowed.
6. Design Exception Class With Test Cases
    * Note: Don't forget to use Singleton Pattern Here.
7. Design Money Splitter
8. Design Notification Service
9. Design Message Queue
10. Design a Terminal/Command Prompt

## 𝐇𝐢𝐠𝐡 𝐋𝐞𝐯𝐞𝐥 𝐃𝐞𝐬𝐢𝐠𝐧 𝐐𝐮𝐞𝐬𝐭𝐢𝐨𝐧𝐬:
1. Design Whatsapp
2. Design Uber Backend
3. Design Tiny URL
4. Design Dream11
5. Design Leetcode
6. Design MS Teams
7. Design Dominos/PizzaHut
8. Design Big Bazaar
9. Design Zomato/Swiggy
10. Design Docker

## 𝐃𝐒/𝐀𝐋𝐆𝐎 𝐐𝐮𝐞𝐬𝐭𝐢𝐨𝐧𝐬:
1. Reverse Vowels in the string.
    * Using Constant space
2. Longest Palindromic Substring.
3. Find the largest island in Matrix of 0s and 1s
4. Longest Repeating Subsequence
5. Compress and Uncompress a Tree/Graph/LinkedList
6. All Variations of Jump Game
7. All Variations of Stocks Buy and Sell DP
8. Word Search 1/2
9. Median of 2 sorted arrays
10. Kth largest/smallest element in the rowwise and columnwise sorted Matrix

Ref: https://leetcode.com/discuss/general-discussion/1893215/repeatedly-asked-microsoft-onsite-questions-ds-lld-hld


### Microservices

Checkout https://medium.com/aspnetrun/microservices-event-driven-architecture-with-rabbitmq-and-docker-container-on-net-968d73052cbb

**Hystrix** is a latency and fault tolerance library designed to isolate points of access to remote systems, services and 3rd party libraries, stop cascading failure and enable resilience in complex distributed systems where failure is inevitable.

**Istio** Service Mesh
![image](https://user-images.githubusercontent.com/19663316/169865776-7a5ec52e-6488-4f2a-a939-a0f35ac0edfa.png)

#### Envoy
**ENVOY** is an Open Source Edge And Service Proxy, Designed for Cloud-Native Applications. 
* Built on the learnings of solutions such as NGINX, HAProxy, hardware load balancers, and cloud load balancers, Envoy runs alongside every application and abstracts the network by providing common features in a platform-agnostic manner. 
* When all service traffic in an infrastructure flows via an Envoy mesh, it becomes easy to visualize problem areas via consistent observability, tune overall performance, and add substrate features in a single place.
* Read more at https://www.getambassador.io/learn/envoy-proxy/
* With the capability of hybrid communication between microservices and API gateway, Envoy proxy helps in handling traffic within data centers (east-west traffic) and also between data centers (north-south traffic). The platform team and network team can easily manage and monitor the traffic for multi-cloud applications.

The notion is to run Envoy as a sidecar next to each service in an application, abstracting the network from the core business logic. Envoy provides features like load balancing, resiliency features such as timeouts, circuit breakers, retries, observability and metrics, and so on. The best part is, one can use Envoy as a network API gateway. These APIs are called discovery services, or xDS for short. 

In addition to the traditional load balancing between different instances, Envoy also allows you to implement retries, circuit breakers, rate limiting, and so on. Also, while doing all that, Envoy collects rich metrics about the traffic it passes through and exposes the metrics for consumption and use in tools such as Grafana, for example.

There are a few alternatives to Envoy proxy, such as Rust Proxy (Linkered is built on it), NGINX Proxy, HAProxy, etc. 

Ref: https://www.tetrate.io/what-is-envoy-proxy/

### Must TODO 

* https://ibm-cloud-architecture.github.io/refarch-integration/journey/
