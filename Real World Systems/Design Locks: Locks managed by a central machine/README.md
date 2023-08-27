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
