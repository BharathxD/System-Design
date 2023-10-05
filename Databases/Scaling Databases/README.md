# Scaling Databases

These techniques are applicable to most databases out there, relational and non-relational

## Vertical Scaling

- Vertical scaling is adding more CPU, RAM, Disk to the existing Database
- Requires downtime during reboot
- Gives you ability to handle “Scale”, more load
- Vertical scaling as a physical hardware limitation

## Horizontal Scaling

- When read:write = 90:10, you move reads to other database
- So that the master db/node is free to handle write operations

![Horizontal Scaling Illustration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Scaling%20Databases/scaling-databases-1.png)

It is important that, API Servers needs to be configured in a way that it should know which database it should connect to

## Replication

Changes on one database (Master) needs to be sent to replica to maintain consistency

### Two modes of replication

**Synchronous Replication**

- Client → API Server → Master DB → Replica
- Only after performing writes on Master DB and Replica, the API Server have to wait
- Strong consistency
- Zero replication lags
- Slower writes

**Asynchronous Replication**

- Client → API Server → Master DB → Replica
- The API Server will receive the response, right after performing the write operation on Master DB (So the API Server need not have to wait for the Master DB to perform replication as it is asynchronous now)
- Eventual consistency
- Some replication lag
- Fast writes

## Horizontal Scaling - `Sharding`

- Because one node cannot handle the data/load, we split it into multiple exclusive subsets that writes on a particular row/document will go to one particular shard
- This way we can scale our overall database load
- _Note: Shards are independent no replication between them_
- API Server needs to know whom to connect to, to get things done
- _Note: Some databases has a proxy hat takes care of routing_
- Each shard can have it’s own replica if needed

![Horizontal Scaling [Sharding] Illustration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Scaling%20Databases/scaling-databases-2.png)
