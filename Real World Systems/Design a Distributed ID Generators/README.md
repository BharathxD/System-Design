# Design some Distributed ID Generators


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

## Snowflake at Instagram

---

Requirements

---

- IDs sortable by time
  - For pagination and Filter and Batch processing
- ~64bits to be efficient on index
- No new service

Structure

---

- 41 Bits - Epoch since Jan 1, 2011
- 13 Bits - DB shard ID
- 10 Bits - Per shard sequence number

Instagram uses a technique called snowflaking in the database during the INSERT process. This technique is used to meet certain requirements that are essential for efficient processing of data. One of the key requirements is the ability to sort IDs by time. This feature is important for pagination, filtering, and batch processing. In order to ensure that the IDs are sortable, they need to be ~64bits in size. This allows for efficient indexing of the data.

Another important requirement is to avoid introducing any new services. This means that the snowflaking technique needs to be implemented within the existing infrastructure.

The structure of the snowflake ID is composed of three parts. The first part is the epoch since Jan 1, 2011, which is represented by 41 bits. The second part is the DB shard ID, which is represented by 13 bits. Finally, the per shard sequence number is represented by 10 bits.

Instagram has implemented logical shards, which are distributed across physical DB servers. For example, a MySQL server is used as the physical server, and each database created on that server is considered to be a logical shard. By using this approach, Instagram is able to effectively manage large amounts of data while maintaining high levels of performance.

Instagram has logical shards (1000s) on Physical DB Servers (10/15)

EG: MySQL Server → Physical & CREATE DATABASE → Logical

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-6.png)

```sql
CREATE DATABASE insta5;
\c insta5;
CREATE SEQUENCE insta5.photos_id_seq;
CREATE TABLE insta5.photos (
    id bigint DEFAULT nextval('insta5.photos_id_seq'),
    -- Other columns in the table
);
CREATE OR REPLACE FUNCTION
  insta5.next_id(OUT result bigint) AS $$
DECLARE
    epoch bigint := 1314220021721;
    seq_id bigint;
    now_ms bigint;
    shard_id bigint := 5;
BEGIN
    SELECT nextval('insta5.photos_id_seq') % 1024 INTO seq_id;
    now_ms := extract(epoch from now()) * 1000;
    result := (now_ms - epoch) << 23;
    result := result | (shard_id << 10);
    result := result | seq_id;
END;
$$ LANGUAGE plpgsql;
```

First, we're creating a brand new database called "insta5" where we'll store all sorts of data.

Next, we're setting up a specific table within this database named "photos." This table will have several columns, but they're not shown here.

Now, let's talk about the interesting part – a function called "insta5.next_id." This function is designed to generate unique identifiers for records in our "photos" table.

Inside this function:

- We have a variable called "epoch," which represents a point in time (1314220021721 milliseconds since some reference point).
- Another variable called "shard_id" is set to 5.
- We calculate a "seq_id" by taking the remainder of a certain sequence value divided by 1024. This "seq_id" will help make our identifiers unique.

Then, we calculate the final "result" identifier:

- First, we figure out the current time in milliseconds, which is not shown here.
- We subtract the "epoch" time from the current time and shift the result left by 23 bits.
- We also incorporate the "shard_id" by shifting it left by 10 bits.
- Finally, we combine everything to create a unique identifier for our records.
