# Non-relational databases

<aside>
💡 Most Non-relational databases shard horizontally out-of-the-box
</aside>

## Three most important Non-relational databases

### **DocumentDB**

- These are closest to relational databases, mostly JSON based
- Supports complex queries (Almost like relational SQL databases)
- MongoDB and Elasticsearch (Although it’s use case is different, but can be used as nosql database) are two popular examples
- Partial updates to the document are possible
  ```json
  {
    "user_id": "1iondf937lihe907sv",
    "total_posts": 270
  }
  ```

### **Key Value Stores**

- These are extremely **simple databases**
- Limited functionalities (GET(K), POST(K, V), DEL(K))
  - K - Key
  - V - Value
- Meant for key-based access patterns
- Do not support complex queries (like aggregations, as compared to DocumentDBs or Relational Databases)
- Can be heavily sharded and partitioned
- You can use relational databases and document db as **KV Stores**
- Use Cases are: profile data, order data, auth data
- Examples: Redis, DynamoDB and Aerospike

### **Graph Databases**

- What if our graph datastructure had a database?
- It stores the data that are represented as nodes, edges and relations
- Example: Neptune, DGraph, Neo4J
- Eg: A -*follows*→ B (or) Bharath -_bought→_ Macbook
- Great for running complex graph algorithms
- Powerful to model social networks, recommendations & fraud detection
