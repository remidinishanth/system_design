REF: https://www.slideshare.net/oemebamo/introduction-to-memcached and https://serhatgiydiren.github.io/system-design-interview-distributed-cache

## Caching
* A copy of real data with faster(and/or cheaper) access

Simple Key/Value Store with the following operations
* save
* get
* delete

### Functional Requirements
* put(key, value) : Stores object in the cache under some unique key
* get(key) : Retrieves object from the cache based on the key

### Non-Functional Requirements
* Scalable (scales out easily together with increasing number of requests and data) : High scalability will help to ensure our cache can handle increased number of put and get requests. And be able to handle increasing amount of data we may need to store in the cache.
* Highly Available (tolerates hardware / network failures, no single point of failure)
* Highly Performant (fast put / get)

### Terminology
* Storage cost
* Retrieval cost (network load/algorithm load)
* Invalidation(Keeping data up to date/removing irrelevant data)
* Replacement policy(FIFO, LRU, LFU, MRU, Random vs Beladys algorithm)
* Cold cache/ Warm cache

* Cache hit and cache miss
  - hit_ratio (hits / hits + misses)
  - miss ratio ( 1 - hit_ratio) 
  - Cache are only efficient when benefits of faster access outweight overhead of checking and maintaining cache(keeping cache up to date) => More cache hits then cache misses


### Cache access patterns:
* Write through: whenever any “write” request comes, it will come through the cache to the DB. Write is considered successful only iff data is written successfully in the cache and in DB.
* Write around: Write request goes around the cache straight to DB and acknowledge is sent back, data is not sent to cache. Data is written to the cache when there is the first cache miss.
* Write back: Write update goes to cache, data is saved in the cache, and acknowledge is send immediately. and then there is one more service that will sink the data to DB. Or say while replacing this entry, we can write to the db.


### Working with HashTable:
* Given a key, we can find the hash function and take modulo to find the bucket/slot in which we have to put this entry. If there are collisions, we can resolve it with separate chaining, open addressing, robin hood hashing.


### Collision Resolution Techniques
* Separate chaining technique creates a linked list to the slot for which collision occurs. Rehash data when the load factor(Number of elements/size of hash table) is too high.
* Open Addressing: Unlike separate chaining, all the keys are stored inside the hash table. Probing is performed until an empty bucket is found. Once an empty bucket is found, the key is inserted.
  - Linear Probing: When collision occurs, we linearly probe for the next bucket until an empty bucket is found.
  - Quadratic Probing: Quadratic probing operates by taking the original hash index and adding successive values of an arbitrary quadratic polynomial until an open slot is found. Example sequence: H+1^2, H+2^2, H+3^2, H+4^2,..., H+k^2
  - Double hashing: We use another hash function hash2(x) and look for i * hash2(x) bucket in ith iteration.
  - Deletion is difficult in open addressing, will need to rearrange few keys after deletion, may be we will need to use dead flag(we can insert new elements in this slot).

![image](https://user-images.githubusercontent.com/19663316/147113662-bdeb0167-97d6-4e6e-a713-abb24d333221.png)
