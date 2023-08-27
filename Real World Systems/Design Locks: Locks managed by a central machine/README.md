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
