# Sharding and Partitioning

**Sharding:** Method of distributing data across multiple machines

**Partitioning:** Splitting subset of data within the same instance

### Understand using a scenario

- Suppose we have created a database that receives 100 WPS (Writes per second)
- And the latter on we will receive a 200 WPS, inorder to handle those writes we can vertically scale database (Add more CPU and RAM)
- And then eventually the writes reaches 1000 WPS, but VERTICAL SCALING IS LIMITED
- In that case horizontal scaling is our only resort, so we will add another DB Instance and redirect half of the writes (i.e: 50% of the write requests) to that DB Instance, thereby reducing the load on the current DB Instance
- **Now,** let’s just understand what a **Shard & Partion** is:
  - A new Database instance that we have created is known as a **SHARD**
  - And data within that instance is being \***\*\*\*\*\***\*\*\***\*\*\*\*\***PARTITIONED\***\*\*\*\*\***\*\*\***\*\*\*\*\***

> Each Database server is thus a shard
> and we say that the data is partitioned

- Overall, a database is `sharded` while the data is `partitioned`

<aside>
💡 Most people use these terms interchangably
</aside>

---

Let’s say that you have partitioned he 100GB of total data into 5 mutually exclusive partitions

![API Server should have the business logic to know which shard it should communicate with (Can use a proxy too)](../../Images/Sharding%20and%20Partitioning/sharding-partitioning.png)

API Server should have the business logic to know which shard it should communicate with (Can use a proxy too)

### How to partition the data

---

- There are two categories of partitioning
  1. **Horizontal Partitioning** (It is **`very common`**)
     - Let’s say, within one table we can pick some rows and put it into one database and pick other rows and put it into other database
  2. **Vertical Partitioning** (It is very common in **`monlith`** and **`microservices`**)
     - In this case, we need One Table in One Database, and Other Table in Other Database
- When we “Split” the 100GB data, we could have used either of the ways but deciding which one to pick depends

## Advantages and Disadvantages of Sharding

| Advantages                                                                                                     | Disadvantages                        |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| Handle large reads and writes                                                                                  | Operationally Complex                |
| Increase overall storage capacity                                                                              | Cross Shard Queries can be expensive |
| (Although the data can be partitioned in a way that there won’t be any need to to perform corss shard queries) |                                      |
