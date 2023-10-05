# Populating the Cache

### Lazy Population (Popular)

- First go to the cache if data exists, return the data
- Or else, Go to the database/ do heavy operation
- Persist in the cache
- Finally, return the data
- Eg: Caching Blogs

![Demonstration of Lazy Population](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Caching/lazy-population.png)

### Eager Population

---

- Writes go to both database and cachein the same request call
- Eg: Live Cricket Score
- Proactively push the data to the cache, because you anticipate the need
- Eg: You know that a popular person’s tweet will most likely br accessed frequently, so you cache the tweet

![Demonstration of Eager Population](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Caching/eager-population.png)
