# Orchestrators

Keeps an eye on the servers. when one server goes down, the orchestrator will spin up another server and put it behind the load balancer

![Leader Election Demonstration](../Images/Orchestrators/leader-election.png)

If the leader orchestrator fails, then one of the worker orchestrators will become the leader orchestrator (Worker Orchestrator Algorithm)
