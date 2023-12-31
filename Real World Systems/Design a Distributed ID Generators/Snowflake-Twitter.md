# Snowflake at Twitter

Twitter, being a huge social media platform, faced the challenge of generating unique IDs for millions of tweets while making sure it was scalable and efficient. To solve this, Twitter came up with a clever approach that combines timestamps and worker IDs.

At the heart of Twitter's ID generation system is something called Snowflake.

(FYI "Snowflake" is not a company, it refers to the uniqueness of different snowflakes) Snowflake assigns 41 bits for timestamps, 10 bits for worker IDs, and 13 bits for the sequence number. By combining these elements, Twitter achieves a balance between uniqueness, ordering, and scalability.

The timestamp in Snowflake represents the time when a tweet is created, making it easy to sort and retrieve tweets in chronological order. The worker ID ensures that each tweet is associated with a specific worker or machine.

To create a unique ID, Twitter's system combines the current timestamp, the worker ID, and a sequence number (counter). The sequence number increases for each tweet generated within the same millisecond, ensuring that even tweets created at the same time have different IDs.

With this carefully designed ID generation system, Twitter is able to handle the huge scale of their platform while maintaining the integrity and uniqueness of every tweet.

Used for IDs of Tweets (also adopted at twitter and instagram)

Snowflakes are 64 Bits integers

![ID distribution demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Distributed+ID+Generators/didg-4.png)

Snowflake is not a central service, instead it runs app service as a native function

![ID distribution demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Design+a+Distributed+ID+Generators/didg-5.png)

**Snowflakes are roughly sortable.**

The first 41 bits represent epoch milliseconds, and with each millisecond, the ID moves forward. This allows us to retrieve objects before or after a certain time.

Twitter's pagination is not limit/offset based. Instead, it uses 'since_id' to paginate.
