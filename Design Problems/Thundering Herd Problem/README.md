# Thundering Herd Problem

Say user A makes an api call to the backend and there was some issue with network and hence the call failed! What do we do?

We retry, assuming out APIs are idempotent (Achieve same results no matter how many times you try)

The already overwhelmed server, is getting hit by all the clients who are retrying again and again

### But how should we retry?

**Implement Exponential Backoff**

Instead of reporting immediately back-to-back we retry with a backoff

This gives server a breathing space and the much needed time to recover

But these retries will still coincide → Which puts pressure on the server

---

**You see this in action on gmail and slack when internet goes down**

We can implement, Exponential Backoff + Jitter (Random)

- Instead of retrying at immediate instant, we add some random Jitter and this randomness (delay) reduces the coincidences during retries
- This ensures retries are distributed and does not add to the problem

---

**While implementing retries ensures**

- You add random filter
- Retries are exponentially spaced
