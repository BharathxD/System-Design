# Snowflake at Instagram

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

![ID distribution demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design%20a%20Distributed%20ID%20Generators/didg-6.png)

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
