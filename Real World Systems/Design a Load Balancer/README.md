# Design a Load Balancer

- Requirements
  - Balance the load
  - Tunable Algorithm
  - Scaling beyond one machine
- Terminology
  - Load Balancer Server
  - Backend Server
- Brainstorm
  - Load Balancer Configurations
  - Monitoring
  - Availability
  - Extensibility

### Load Balancing Algorithms

---

**Round Robin [Uniform Infrastructure]**

---

- Distribute the load `Iteratively`
- For Homogenous infrastructure, where every server has same hardware configuration [Hypothetically]

**Weighted Round Robin [Non-uniform infrastructure]**

---

- Distribute the load iteratively but as per `weights`
- Heterogenous Infrastructure

**Least Connections**

---

Pick the server having the `least connections` from the load balancer

**Hash Based Routing** (`Random`)

---

Hash of some attribute (ip, userId, url) determines which server to pick (You can configure `stickiness`)

---

![Demonstration of low-level architecture of Load Balancer](../../Images/Load%20Balancer/lb-lowlevel-arch.png)

The information about the backend server will be stored in the load balancer server memory. Initially, the load balancer server will pull the details about the backend servers when it is initialised. Additionally, the load balancer server will be subscribed to a pub/sub so that it can be updated every time there is a change in the backend servers.

### Databases

---

- To store the backend server details, a key-value store like Redis can be used
  - KVDB server is subscribed to Redis pub/sub, so that whenever the db is updated we can make LB Servers update their in-memory data about backed servers
- Prometheus can be utilised to store meta information about the servers, allowing the autoscaling service to determine when scaling is necessary.

### Orchestrator

---

The load balancer is prone to a single point of failure. To overcome this issue, we can use an orchestrator that regularly keeps track of the LB servers. If one server goes down, the orchestrator can deploy another LB. (The LB server should send frequent pulse requests to the orchestrator)
