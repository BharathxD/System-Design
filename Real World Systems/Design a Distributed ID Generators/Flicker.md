# How Flickr did it?

Why did Flicker need its own ID generation?

- The database was sharded.
- To avoid collisions and guarantee uniqueness.

Why are they building their own ID system if UUID already exists?

- UUIDs are at least 128-bit (16 bytes) integers.
- They are very inefficient at scale.
  - They do not index well.
  - They bloat the index as the database scales.
- Although they are random, which is good for security, disk-heavy index lookups cause significant database performance issues.

So why is MongoDB using a 12-byte ObjectID?

![ID distribution demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Distributed+ID+Generators/didg-1.png)

They also do not index well!

<aside>
💡 If you cannot fit the indexes in-memory, you cannot make your DB fast

</aside>

### **Central Ticker Server (MySQL)**

---

- Whoever wants an ID, fires a query to this database

```sql
CREATE TABLE `query` (
	id bigint unsigned not null auto_increment,
	stub char not null default ` `,
	PRIMARY KEY(id),
	UNIQUE KEY(stub)
)
```

### Idea

---

Delete and re-insert the row will make DB generate the next id (auto-incremented)

1. We can either delete and re-insert in one transaction, but it is extremely costly on the DB
2. UPSERTS (Delete + Inserts = Upsert)

MySQL provides 2 ways of doing it

```sql
INSERT … ON DUPLICATE KEY UPDATE …
```

Tries to insert, if it fails due to UNIQUE KEY then the update clause is expected

```sql
REPLACE INTO … ← This is 32x slower compared to ON DUPLICATE
```

Tries to insert, if it fails due to UNIQUE KEY then the old row is deleted and new row is inserted

```sql
INSERT INTO `tickets` (stub) VALUES ('a')
ON DUPLICATE KEY id = id + 1;
```

![ID distribution demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Distributed+ID+Generators/didg-2.png)

### Caution: SPOF!

---

It is prone to Single Point of failure as we habe only one database

There are two ticket servers:

- One is for odd numbers
- The other is for even numbers

To balance the load, use a load balancer with a Round-robin algorithm.

**Ticket Server 1**

---

- Auto-increment increment: 2
- Auto-increment offset: 1

**Ticket Server 2**

---

- Auto-increment increment: 2
- Auto-increment offset: 2

![ID distribution demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Distributed+ID+Generators/didg-3.png)

If one server goes down, the other server continues to serve. For example, 2, 4, 6, 8, …., 200, 202. When the odd server comes back, we reset the offset on both databases by MAX + Buffer. For instance, 240 and 241.
