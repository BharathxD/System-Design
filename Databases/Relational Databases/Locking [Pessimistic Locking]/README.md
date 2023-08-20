# Database Locking [Pessimistic Locking]

Core Idea: You acquire the lock before proceeding

Typical Flow

- ACQ_LOCK()
- READ/UPDATE
- REL_LOCK()

**Two types of locking strategies**

- Shared Lock
- Exclusive Lock

**Why do we need locks**

- To maintain consistency and integrity of the data

**The risk here is (Whenever we add pessimistic locking)**

- `Transactional Deadlock`
- The transaction that detects the deadlock kills itself

![TRANSACTIONAL DEADLOCK](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb061f4c-d682-4899-8d9d-c5ceb3c510d9/Screenshot_2023-08-19_at_3.19.10_PM.png)

TRANSACTIONAL DEADLOCK

### Shared Locks (Read Locks)

- Reserved for `read` by the current transaction
- Other transactions can `read` the locked rows
- Other transactions cannot `modify` the locked rows
- If the current transaction wants to `modify` then the locks will be upgraded to an `exclusive lock`

### Implementation of Shared Locks

We can fire an SQL query with `FOR SHARE` at the end, to trigger shared lock

```sql
SELECT * FROM .... FOR SHARE;
```

| id  | user_id                   | …   |
| --- | ------------------------- | --- |
|     | 1 [T1]                    |     |
|     | 2 [T1]                    |     |
|     | 3 [T2]                    |     |
|     | 4 [T2]                    |     |
|     | 5                         |     |
|     | 6 [T1] [T2 - READ & Wait] |     |

**Transaction 1 (`Read` & `Modify`)**

```sql
SELECT .... WHERE user_id IN (1, 2, 6) FOR SHARE;
```

**Transaction 2 (`Read` & `Wait`)**

```sql
SELECT .... WHERE user_id IN (3, 4, 6) FOR SHARE;
```

### Exclusive Locks

- Reserved for `write` by the current transaction
- Other transactions cannot `read & Modify` the locked rows

### Implementation of Exclusive Locks

We can fire an SQL query with `FOR UPDATE` at the end, to trigger shared lock

```sql
SELECT * FROM .... FOR UPDATE;
```

| id  | user_id            | …   |
| --- | ------------------ | --- |
|     | 1 [T1]             |     |
|     | 2 [T1]             |     |
|     | 3 [T2]             |     |
|     | 4 [T2]             |     |
|     | 5                  |     |
|     | 6 [T1] [T2 - Wait] |     |

**Transaction 1 (`Read` & `Modify`)**

```sql
SELECT .... WHERE user_id IN (1, 2, 6) FOR UPDATE;
```

**Transaction 2 (`Wait`)**

```sql
SELECT .... WHERE user_id IN (3, 4, 6) FOR UPDATE;
```

### SKIP LOCKED

Removes the locked rows from the result set

```sql
SELECT * FROM t WHERE id = 2 FOR UPDATE SKIP LOCKED;
```

### NOWAIT

- Locking read does not wait for the lock to be acquired.
- It `fails immediately` if the row is locked

```sql
SELECT * FROM t WHERE id = 2 FOR UPDATE NOWAIT;
```

`ERROR 3572: DO NOT WAIT FOR LOCK`
