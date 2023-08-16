# Online/Offline Indicators

`PS:` Let’s not use Web Sockets in this Approach (irl, we use web-sockets)

`Similar System:` Failure Detection in a Distributed System

### Interfacing API

---

- Make a HTTP `GET` request to the API server:
  - GET status/profile/?userId=u1,u2,u3,u4

### Updating the Database

---

- Periodically send HTTP `POST` requests to the API Server, to let API Server know that you’re online
- It’s like a pulse, as long as you send the requests you’re online
- User (/pulse) - - - - - → API Server

### Updating the Database when the user is offline

---

- When we do not receive the pulse long enough handling throught the business logic (let’s say that 30Seconds)
- In the database we store, ‘Time you received the last pulse’
  | user_id | last_pl |
  | ------- | ------- |
  | u1 | 1000 |
  | u2 | 1050 |
  | u3 | 1060 |
  When you receive the pulse:
  ```sql
  UPDATE pulse SET last_pl=Now() WHERE user_id='u1'
  ```
- Let’s assume that the user sends pulse every `10 Seconds`

### Get Status API

---

- GET status/profile/?userId=u1,u2,u3
  - If there is no entry in the database for the user → OFFLINE
  - If entry and entry.last_pl < Now() - 30 Seconds → Offline

### Lets estimate the scale

---

- 100 Users → 100 Entries
  1000 Users → 1000 Entries
  1 Million Users → 1 Million Entries
  1 Billion Users → 1 Billion Entries
- Each entry has 2 Columns
  - user_id → INT(4B)
  - last_pl → INT(4B)
  - So, the size of each entry is 8B
- Total Storage required for all 1B Users/Entries out there is: 8B \* 1 Billion → **`8GB`**

### Can we do better on storage?

---

- Once the user is offline (Assuming that we the API Server didn’t receive any request/pulse from the client under 30Seconds), we will delete that entry from the database saving us more space

### How to auto delete the expired entries

---

- **Approach 1**: Write a CRON Job that deletes expired entries (`Absolutely DON'T`: we don’t need the extra service that is running and doing those tasks)
- **Approach 2**: Can we not offload this task to a datastore?

### Catching up with Approach 2

---

- We need a **DATABASE** with KV + Expiration
  - Redis
  - DynamoDB

### Which Database am I picking and why?

---

| Features             | Redis | DynamoDB |
| -------------------- | ----- | -------- |
| Persistance          | ❌    | ✅       |
| Managed Service      | ❌    | ✅       |
| No Vendor Lock-In    | ✅    | ❌       |
| Future Extensibility | ✅    | ❌       |
| Time Sensitivity     | ✅    | ❌       |

### How is our Database doing

---

- Pulse every 10 Seconds
- So, one user sends 6 requests every one minute
- `1 Request = 1 DB Call`
- If there are 1M active users, then there will be `6M Requests/min`

### How to make it better?

---

PS: Connection pool is not a service, it’s simple library that has been set up on the API Server

Creating connection every time is a `hectic part` here (And also the computation)

(3 Way handshake to create a connection, and 2 way handshake for the tear-down)

- We add a `connection pool` to make it perform better

Server -connection_pool→ Database

Pre establishing the connection, we are saving the bandwidth, cpu, network
