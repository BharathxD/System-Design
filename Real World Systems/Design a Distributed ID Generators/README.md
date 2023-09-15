# Design some Distributed ID Generators

## How Flickr did it?

---

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

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-1.png)

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

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-2.png)

### Caution: SPOF!

It is prone to Single Point of failure as we habe only one database

## Snowflake at Twitter

---

Twitter, being a huge social media platform, faced the challenge of generating unique IDs for millions of tweets while making sure it was scalable and efficient. To solve this, Twitter came up with a clever approach that combines timestamps and worker IDs.

At the heart of Twitter's ID generation system is something called Snowflake.

(FYI "Snowflake" is not a company, it refers to the uniqueness of different snowflakes) Snowflake assigns 41 bits for timestamps, 10 bits for worker IDs, and 13 bits for the sequence number. By combining these elements, Twitter achieves a balance between uniqueness, ordering, and scalability.

The timestamp in Snowflake represents the time when a tweet is created, making it easy to sort and retrieve tweets in chronological order. The worker ID ensures that each tweet is associated with a specific worker or machine.

To create a unique ID, Twitter's system combines the current timestamp, the worker ID, and a sequence number (counter). The sequence number increases for each tweet generated within the same millisecond, ensuring that even tweets created at the same time have different IDs.

With this carefully designed ID generation system, Twitter is able to handle the huge scale of their platform while maintaining the integrity and uniqueness of every tweet.

Used for IDs of Tweets (also adopted at twitter and instagram)

Snowflakes are 64 Bits integers

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-4.png)

Snowflake is not a central service, instead it runs app service as a native function

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-5.png)

**Snowflakes are roughly sortable.**

The first 41 bits represent epoch milliseconds, and with each millisecond, the ID moves forward. This allows us to retrieve objects before or after a certain time.

Twitter's pagination is not limit/offset based. Instead, it uses 'since_id' to paginate.

## Snowflake at Discord

---

Same logic as twitter, just epoch = 1st second of 2015 (To get a larger range)

## Snowflake at Sony

---

They call it Sonyflake and their Go-based implementation is open-sourced and can be found on Github
