# Non-relational Databases

Data in non-relational-ly structured

- In most cases, NoSQL databases provide scalability and availability by compromising consistency (IN MOST CASES, NOT ALWAYS)
- Non-relational database shard horizontally out-of-the-box

Most NoSQL Databases are eventually consistent

## Types of NoSQL Databases

### **DocumentDB**

- Mostly JOSN based
- Supports complex queries
- Partial Updates to Documents possible
- Closest to Relational Database
- EG: MongoDB, Elasticsearch

### Key Value Store

- Key-wise access pattern
- Heavily Partitioned
- No Complex Queries Supported
- EG: Redis, DynamoDB, Aerospike

### Column Oriented Databases

- Column oriented databases will read columns that are part of the query and will not even skim others
- EG: In a row-oriented databases, per say if we want to filter out the data that is related to only one column, the database have to read entire row even though it is not necessary in our case: but the column oriented databases will read and filter out only the particular column that the user mentioned in the query

```sql
SELECT avg(price) WHERE ts='____'
```

- All you care is two columns `price` and `ts`, but a row DB will go row by row reading all columns & discarding 98% of them (Which is inefficient)
- Is is a classic case of `Analytics Query`
- Hence, column oriented DB are used in Massive Analytics & Warehouses. EG: Redshift
- Foundational Paper on Column Oriented DB
  - C-Store: A Column Oriented DBMS

### Graph Databases

- Stores data in nodes and edges
- Great for modelling `social behaviours`, `recommendations`
- Solid use cases are: Fraud Detection
- EG: Neo4J, Neptune, DGraph, TigerGraph
