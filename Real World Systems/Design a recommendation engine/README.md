# Design a recommendation engine

![Architecture 1](../../Images/Design%20a%20recommendation%20engine/recommendation-engine-1.png)

**Cosine Similarity [Exploitation]**

---

- Convert product into vectors in ‘n’ dimensional space where ‘n’ = features like tokens, price, category, etc

![Architecture 2](../../Images/Design%20a%20recommendation%20engine/recommendation-engine-2.png)

- Product A and B are more similar than A & C
- We know `cos(0)=1` and `cos(90)=0`
  - hence, `similarity = cos(0)`

**Collaborative Filtering [Exploration]**

![Architecture 3](../../Images/Design%20a%20recommendation%20engine/recommendation-engine-3.png)

- Collaborative filtering clusters users and recommends things that other similar user bought
- eg:
  - A <bought> iPhone
  - A <similar to> B
  - B <could buy> iPhone (recommendation)
- core idea: Of all the missing edges, which one has the maximum probabilities of occuring
- Instead of reccomending similar items, collaborative opts exploration
- This approach helps to spice up reccomendations
