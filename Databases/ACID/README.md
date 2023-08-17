# ACID

- Relational databases provides **Transactions**
  - **Atomicity** - All the statements within a transaction takes effect or none
  - **Consistency** - Data will never go wrong no matter what constraints, cascades, triggers
    - example: Foreign key checks do not allow you to delete parent if child exists
  - **Isolation** - When multiple transactions are executing parallely, the isolation level determines how much changes of one transaction visible to other
  - **Durability** - When transaction commits, e changes outlives outage
- It follows ACID Properties - Atomicity → Consistency → Isolation → Durability
