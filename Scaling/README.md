# Scaling

Ability to handle large number of **`concurrent`** requests

---

### Vertical Scaling

---

Make infrastructure bulky add more CPU, RAM, Disk

- **Pro:**
  - Eazy to manage
- **Con:**
  - Risk of downtime
  - Hardware Limitation

### Horizontal Scaling

---

Make infrastructure bulky add more CPU, RAM, Disk

- **Pro:**
  - Linear Amplification
  - Fault Tolerance
- **Con:**
  - Network Partitioning
  - Complex Architecture

### Good Scaling Plan (In General)

---

Scale the server vertically initially and then eventually scale it horizontally

<aside>
💡 Load testing is the only accruate way to find out how many requests a server can handle

</aside>

---

If you horizontally scale the system, then the other services like, Database, centralized cache and other services. would go down

<aside>
⚠️ Whenever you are scaling ensure that you’re doing **BOTTOM-UP** scaling

</aside>

### Scaling Databases

---

- For a read-heavy system like Youtube, you need to use read-replicas. So that all the read requests would go to the read replicas. (Reading the transactions would need to go to the masterdb, for other informations such as profile information would need to go to the read replicas)
- When one node of the masterdb is not able to handle the load, we can **shard** the database
