# Caching at different levels

### CDN - Content Delivery Networks

CDNs are a set of servers distributed across the world. Request from a user, goes to the nearest CDN server and hence user gets very quick response, people residing in US requestiing media files from US Servers is faster than fetching it from India

- CDN does [lazy cache](https://www.notion.so/Caching-f684b455dde44a4e9efad72df637ae08?pvs=21) population

**Client ←→ CDN ←→ API**

### Remote Cache (Redis)

Remote cache is centralized cache that we most commonly use (Redis). Multiple API servers use it to store infrequently accessed data

- Every key that is stored should have an expiration (Memory Leak)
- Size of cache is relatively very small as compared to a database
- I tis expensive and stores data in main memory

### Database Caching

Instead of computing total posts by users everytime

We Store `total_posts` as comlumn and update i once a while. (This save an expensive db computation)

### Other ways to cache

NoteL There are other places like load balancer where we can cache

- We can cache some data at every single component in the system but should we do it?
  - Not necessarily: It is very use-case specific and subject to tolerance level of staleness of the served data

> Just because you can does not mean you should
