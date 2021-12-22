REF: https://aaronice.gitbook.io/system-design/architecture-toolbox/id-generator

### Requirements
Usually there are a few requirements for ID generators:
* They cannot be arbitrarily long. Let’s say we keep it within 64 bits.
* ID is incremented by date. This gives the system a lot of flexibility, e.g. you can sort users by ID, which is same as ordering by register date.


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

Limitation for counter: since the ID cannot be arbitarily long, the counter may end up with only 8bits for instance. In this case, the server can only handle 256 requests within a single timestamp at most. -- If it frequently exceeds this limit, we need to add more instances.

Note:
These ids need to be roughly sortable, meaning that if tweets A and B are posted around the same time, they should have ids in close proximity to one another since this is how we and most Twitter clients sort tweets.

### Problem: Clock Synchronization

There’s a hidden assumption that all ID generation servers have the same clock to generate the timestamp, which might not be true in distributed systems. In reality, system clocks can drastically skew in distributed systems
