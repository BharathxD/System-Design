# Load Balancing Algorithms

**Round Robin**: Distributes the load iteratively.

**Weighted Round Robin**: Distributes the load iteratively based on weights.

**Least Connections**: Selects the server with the fewest connections from the load balancer.

**Hash-Based Routing**: Uses a hash of some attribute (IP, userId, URL) to determine which server to pick. You can configure stickiness for this.
