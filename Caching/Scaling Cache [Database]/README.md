# Scaling

<aside>
💡 Cache is just like a database, hence scaling technique for a cache like redis is similar to regular database

</aside>

### **Vertical Scaling**

- Make you cache bigger handle more data/load

### **Horizontal Scaling - Replica (Scaling Reads) (A Ready Replica)**

- Same data replicated across ultiple nodes so at reads could scale

### **Horizontal Scaling - Sharding (Scaling Writes)**

- Data partitioned across multiple hards, so that writes could scale, each shard can have a replica
- Shards are mutually exclusive
