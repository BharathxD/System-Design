# Big Data Processing

- When one machine is not enough to process the data We divide and conquer → This essentially is Big Data Processing
- Companies use this to process massive amount of data and extract insights out of it, train ml models, move data across databases, and much more
- When too much data needs to be processed quickly, we can use big data tools
- All these fancy processing on commodity hardware (These are not specialized hardware)

### Distributed Computing

More computers, more cpu, more processing

- ‘Split’ the file into the ‘partitions’
- Distribute the partitions across all the servers
- Let each server compute the frequency independently
- Send the word frequency to one server (Coordinator) merge the word frequencies

### Flow

![Flow Demonstration](../Images/Big%20Data%20Processing/flow.png)

1. User submits the job to ‘coordinator’
2. Coordinator distributes the job across multiple machines
3. Machines (Worker) compute and send result to coordinator
4. Co-ordinator merges and returns

**Challenges**

- What about failures?
- What about recovery?
- What about completion?
- What about scaling & distribution?

**So how?** That’s what **Big Data** tools are for

---

Large scale data processing on commodity hardware it has connectors to a lot of databases and infra components

![Spark Operation](../Images/Big%20Data%20Processing/spark.png)

**eg**: combine user, order, payments and logistics DBs and put the result in AWS Redshift
