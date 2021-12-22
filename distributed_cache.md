REF: https://www.slideshare.net/oemebamo/introduction-to-memcached

## Caching
* A copy of real data with faster(and/or cheaper) access

Simple Key/Value Store with the following operations
* save
* get
* delete

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
