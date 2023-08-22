# Thundering Herd Problem

Say user A makes an api call to the backend and there was some issue with network and hence the call failed! What do we do?

We retry, assuming out APIs are idempotent (Achieve same results no matter how many times you try)

The already overwhelmed server, is getting hit by all the clients who are retrying again and again

### But how should we retry?
