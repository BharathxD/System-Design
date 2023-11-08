# Scaling

> 💡 Cache is just like a database, hence scaling technique for a cache like Redis is similar to a regular database.

## Vertical Scaling

When it comes to **Vertical Scaling**, the idea is to make your cache bigger to handle more data and load.

## Horizontal Scaling - Replica (Scaling Reads) (A Ready Replica)

For **Horizontal Scaling with Replica (Scaling Reads)**, the approach is to have the same data replicated across multiple nodes. This allows you to scale your read operations efficiently.

## Horizontal Scaling - Sharding (Scaling Writes)

With **Horizontal Scaling with Sharding (Scaling Writes)**, data is partitioned across multiple shards, enabling write scaling. Each shard can also have replicas, and the shards are mutually exclusive.

The choice between vertical scaling, horizontal scaling with replicas, and horizontal scaling with sharding depends on the specific requirements and characteristics of your application.
