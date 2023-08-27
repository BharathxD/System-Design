# Design Locks: Locks managed by a central machine

- **Brainstorm**
  - Locking
  - Core Properties

The 3 machines co-ordinate through a central lock manager

- Multiple Threads synchronise through
  - Mutex & Semaphore
  - Disk
  - Remote Lock

Example: ‘apt-get upgrade’ cannot be run twice concurrently

---

To understand remote locks better, let’s synchronise multiple consumers over an unprotected remote queue

![Abstract Architecture of lock](../../Images/Locks/abstract-lock.png)

Queue is unprotected, we want one consumer to make call to the queue at a time (Not all consumers can read at a time)

---

A consumer can pick the item from the queue, update the database in order to lock, so when consumer x finishes the processing it can release the lock by updating the database

- Database Requirements
  - A key-value store
  - TTL
  - Atomic Database

Redis would be the Database

- It is a KV Store
- It provides RedLock
- Also has a feature called TTL

![Example with Database Involved](../../Images/Locks/lock-database.png)
