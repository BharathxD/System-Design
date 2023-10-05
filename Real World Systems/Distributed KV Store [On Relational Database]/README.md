# Distributed KV Store [On Relational Database]

**Requirements**

- Infinitely Scalable
- GET, PUT, DEL, TTL

**Brainstorm**

- Storage
- Optimize Storage
- Insert
- Update
- TTL

### Schema

| key | value | expired_at |
| --- | ----- | ---------- |
| k1  | v2    | ….         |
| k2  | v1    | ….         |

---

**Delete**

```sql
UPDATE store SET expired_at = -1 WHERE key = k
```

**Insert**

```sql
UPSERT INTO store VALUES(key, value, NOW()) (..);
```

`If your database engine supports you can use UPSERT (Postgres Does), REPLACE INTO (MySQL)`

### Implementing TTL

---

**Approach 1:** Batch Deletion with CRON Job

- Hard delete the data on a particular time that we configure
- What about expired keys before they are hard deleted?
  - We will filter them out with an SQL Query

**Approach 2:** Lazy Deletion [Only for in-memory databases]

Do hard delete when an expired key is fetched

- What if the key is never fetched?
  - Key is never deleted
  - CRON to delete expired key [Small Pause]
- You only do hard-deletion in in-mem database, not databases that are on disks [Tree rebalancing]

**Approach 3:** Random Sampling & Deletion

[Not suitable for disk backed databases]

- Randomly sample 20 keys having expiration set
- Delete all the keys that are expired from the sample
- If deleted key > 25%, repeat the process
- **Idea:** If sample has <25% of expired keys, population will have <25% of expired keys (Not Deleted)

<aside>
💡 This approach is used by Redis & the number (sample size) 20 comes from the **[Central Limit Theorem](https://en.wikipedia.org/wiki/Central_limit_theorem)**

</aside>

### Minor Improvements

---

**Implementing Delete**

```sql
UDPATE store SET ttl = -1 WHERE key = k1 AND ttl > NOW()
```

**Implementing Batch cleanup delete**

```sql
DELETE FROM store WHERE ttl <= NOW()
```

**Implementing GET**

```sql
SELECT * FROM store WHERE key = k1 AND ttl > NOW()
```

### High Level Architecture

---

![HLA Arch 1 Demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Distributed%20KV/hla-1.png)

Read from read replicas (If staleness is not a problem)

Or read from Master Node for Strong Consistency

### Scale Master node

---

![HLA Arch 2 Demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Distributed%20KV/hla-2.png)

Each master node owns an exclusive fragment of the data
