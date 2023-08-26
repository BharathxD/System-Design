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
