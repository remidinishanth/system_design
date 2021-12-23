REF: https://aaronice.gitbook.io/system-design/architecture-toolbox/id-generator

### Requirements
Usually there are a few requirements for ID generators:
* They cannot be arbitrarily long. Let’s say we keep it within 64 bits.
* ID is incremented by date. This gives the system a lot of flexibility, e.g. you can sort users by ID, which is same as ordering by register date.

Since it has to be unique: Our intuitation says that we can use time, get every millisecond/microsecond. Can call this time on different computers. What will we mix in IP addr, Ethernet HW addr or CPU serial number. Identity of computer + time. What if we make the call for unique id within same time unit, may be we can use few more bits for a counter. Time doesn't always go forward on computers. NTP to sync time.

Instead of clock synchronization, we can use our own counter which increments for unique ID. We can write our ID only for a thousands of instructions. REF: https://www.youtube.com/watch?v=W6qURtqrldc&t=602s Since clock synchronization is hard, we can seperate this generation to a seperate service.

Use case Flickr: Ticket servers give us globally (Flickr-wide) unique integers to serve as primary keys in our distributed setup. REF: https://code.flickr.net/2010/02/08/ticket-servers-distributed-unique-primary-keys-on-the-cheap/

GUIDs?
Given the need for globally unique ids the obvious question is, why not use GUIDs? Mostly because GUIDs are big, and they index badly in MySQL. One of the ways we keep MySQL fast is we index everything we want to query on, and we only query on indexes. So index size is a key consideration.

Centralizing Auto-Increments, have one MySQL db just for generating IDs.
If we can’t make MySQL auto-increments work across multiple databases, what if we just used one database? This db can become really huge and unmanageable at some point in time. Also this is single point of failure/bottleneck. Two ticket DBs (one on odd numbers, the other on even) to avoid a single point of failure.

### Single machine/Same machine
* Incrementing ID --> Larger ID means the user is registered later.
* What if system goes down? We need to persist the ID which are used, we can do actually store the ID only once per 1000 requests, if system goes in between just increment the stored ID by 1000, so that we always return the unique IDs.

Say we are using more than one machine, then we can have a seperate service.

### Seperate service
* We can keep a single separate machine for ID generation, there will be no risk of generating duplicate IDs, but this service becomes the bottle neck.

### Multiple machine solution

Example:
* Open Source ID Generator - Snowflake from Twitter

How:
Since within a single timestamp there can also be multiple users, we could solve this with two approaches.

* We assign a `server ID` to each ID generation server and the final ID is a combination of `timestamp` and the `server ID`.
* We can also allow multiple requests within a single timestamp on a single server. We can keep a counter on each server, which indicates how many IDs have been generated in the current timestamp. So the final ID is a combination of `timestamp`, `serverID` and the `counter`.

`Final ID = Timestamp + ServerID + Counter`

Limitation for counter: since the ID cannot be arbitarily long, the counter may end up with only 8 bits for instance. In this case, the server can only handle 256 requests within a single timestamp at most. -- If it frequently exceeds this limit, we need to add more instances.

Note:
These ids need to be roughly sortable, meaning that if tweets A and B are posted around the same time, they should have ids in close proximity to one another since this is how we and most Twitter clients sort tweets.

### Problem: Clock Synchronization

There’s a hidden assumption that all ID generation servers have the same clock to generate the timestamp, which might not be true in distributed systems. In reality, system clocks can drastically skew in distributed systems


#### Instagram ID generation:

REF: https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c

Before starting out, we listed out what features were essential in our system:
* Generated IDs should be sortable by time (so a list of photo IDs, for example, could be sorted without fetching more information about the photos)
* IDs should ideally be 64 bits (for smaller indexes, and better storage in systems like Redis)
* The system should introduce as few new ‘moving parts’ as possible — a large part of how we’ve been able to scale Instagram with very few engineers is by choosing simple, easy-to-understand solutions that we trust.

Each of our IDs consists of:
* 41 bits for time in milliseconds (gives us 41 years of IDs with a custom epoch)
* 13 bits that represent the logical shard ID, can be data center id + worker id
* 10 bits that represent an auto-incrementing sequence, modulus 1024. This means we can generate 1024 IDs, per shard, per millisecond
