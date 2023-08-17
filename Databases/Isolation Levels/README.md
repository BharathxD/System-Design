# Isolation Levels

- **Repeatable Reads (`REPEATABLE-READS`)**
  - Consistent reads within same transaction
  - Even if the other transaction has commited the changes, the changes **wouldn’t occur** in the current transaction (Unless the current transaction is commited)
- **Read Commited (`READ-COMMITED`)**
  - Reads within the same transaction always reads the fresh/latest values
  - The only con being, multiple reads within the same transaction is not consistent
  - If the other transaction has commited the changes, the changes **will** occur in the current transaction (Only if the other transaction is commited)
- **Read Uncommited (`READ-UNCOMMITED`)**
  - Reads the uncommited values in the other transaction (Which is not the case with other Isolation Levels)
  - This is called as a “**Dirty Read**“ Problem
  - Gives you slightly higher throughput
- **Serializable** (**`SERIALIZABLE`**)
  - Every read is a locking read (Depends on engine though)
  - While one transaction reads, others will have to wait
  - So basically, every read that you do is locking read
