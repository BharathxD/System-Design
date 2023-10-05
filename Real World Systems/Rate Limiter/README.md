# Design a Rate-limiter

![Arch 1](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Rate%20Limiter/rate-limiter-1.png)

Systems break down under tremendous load, and we need to ensure that it doesn’t happen

**Design a rate limiter, that:**

1. Limits the number of requests in a given period of time
2. Allows developers to configure threshold at a granular level
3. Does not add a massive additional over-head

### Rate limiter

---

- The Rate limiter is the first line of defense
- Any incoming request is first consulted against the rate limiter
- If we are under limits, let the request pass-through
- Otherwise, reject the request with Error **[429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429)** (Too many requests)

### How to implement that?

---

Let’s implement the rate limiter as a library rather than a service

- The library holds all the business logic out there

![Arch 1](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Rate%20Limiter/rate-limiter-2.png)

### How to we scale the Redis DB?

---

- We can scale vertically
- We can add shard to the database
