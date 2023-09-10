# Understanding Data Storage in Relational Databases

In the world of databases, data is organized and presented in a structure that resembles tables with rows and columns. This arrangement simplifies how we work with data and helps us make sense of information.

## The Relational Database and ACID Principles

Relational databases are well-known for adhering to the ACID principles, which ensure data integrity and reliability in transactions. Here's a brief overview:

- **Atomicity**: It ensures that database transactions are treated as a single unit, so they either fully succeed or fully fail, with no partial changes.

- **Consistency**: After a transaction, the database must remain in a consistent state. In other words, it moves from one valid state to another.

- **Isolation**: Transactions should be isolated from each other to prevent interference. Each transaction should execute independently.

- **Durability**: Once a transaction is committed, its changes are permanent and will survive any system crashes.

## Exploring More Topics

To dive deeper into the world of relational databases, explore these topics:

- [Indexes](./Indexes/README.md): Learn about how indexing can improve data retrieval efficiency in databases.

- [Locking](./Locking%20[Pessimistic%20Locking]/README.md): Discover the concept of locking, including pessimistic locking, and how it plays a crucial role in concurrent database operations.
