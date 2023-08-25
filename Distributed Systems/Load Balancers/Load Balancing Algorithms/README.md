# Load Balancing Algorithms

**Round Robin**: Distribute the load `Iteratively`

**Weighted Round Robin**: Distribute the load iteratively but as per `weights`

**Least Connections**: Pick the server having the `least connections` from the load balancer

**Hash Based Routing**: (`Random`) Hash of some attribute (ip, userId, url) determines which server to pick (You can configure `stickiness`)
