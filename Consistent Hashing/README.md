# Consistent Hashing

One of the most amazing and popular algorithm out there and the only problem it solves is data ownership

<aside>
💡 Hashing logic is `NOT` a 'service’, but just a simple code running in the load balancer’s code (It’s a simple function)

</aside>

## Hash Based Ownership

Say, we have a load balancer and when a request comes in, it uses hash of access token to decide which backend server to forward it to

SHA-256 or SHA-128 are popular choices

SH256(u1) = x

server = x % 3 = i

## Load balancers are stateless

1. Because the API servers are stateless.
2. Which means every server is equally capable of handling requests it doesn’t matter if the request that was handled by server 1 now starts going to server 0
3. This is precisely why we see ‘Hash Based Routing’ as one of the most common ways of routing for stateless backends
4. eg: Load Balancer + API Servers

## Hash based ‘Routing’ (Ownership) for distributed storage

![Hash based routing for distributed storage](../Images/Consistent%20Hashing/hash-based-routing.png)

Say we want to share 6 keys: `k1, k2, k3, k4, k5 and k6`

```rs
k1 → fn(k1) % 3 = 2

k1 → fn(k2) % 3 = 0

k1 → fn(k3) % 3 = 1

k1 → fn(k4) % 3 = 2

k1 → fn(k5) % 3 = 1

k1 → fn(k6) % 3 = 0
```

The only challenge is: If a storage node is removed or added, the proxy connot just forward request to any arbitrary node because it won’t have the data

- Now, all the keys would need to be re-evaluated and moved to the correct node
  - It involves a lot of data transfer

## Can we minimize the data movement?

This is where **consistent hashing** comes in

### What is consistent hashing?

Consistent hashing is an algorithm that helps in ddetermining data ownership (In the sense, who owns this data)

### What consistent hashing is not?

- It will not do data transfer for us
- It is not a ‘service’ in itself

## Hash Function (SHA 128)

Given hash functions are cyclic we can visualize it as a ring of integers, every node occupies one slot in the ring, the slot is calculated by passing the node’s IP address

![Circular Hash Demonstration](../Images/Consistent%20Hashing/circular-hash.png)

We don’t need any circular linked-list kind of datastructures here, it’s just a simple array and can be part of a proxy

<aside>
💡 **Recipe**
k1 → hash → 0 → node to the right → which is node 0

k2 → hash → 10 → node to right → which is node 3

</aside>

### Scaling up

When we add a new node to the ‘ring’,

- Say Node 3 (Which is a new node) hashes to slot 1
- The keys that hashed between slot 12 and slot 1 will now be ‘owned’ by node 3 instead of node 0
- Other keys continue to remain at the respective nodes
  - **i.e: Minimal Data Movement**
- Operationally, you just have to:
  1. Snapshot Node 0
  2. Create Node 3
  3. Delete unused keys

### Scaling down

- Say we scale down and remove node 0
- All the keys that were owned by Node 0 will now be owned by Node 2 (next in the ring)
  - **ie: Minimal Data Transfer**
- Operationally, you just have to:
  1. Copy everything from Node 0 to Node 2
  2. Remove Node 0 from the ring
