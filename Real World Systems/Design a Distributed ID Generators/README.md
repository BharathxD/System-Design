# Design some Distributed ID Generators

## Snowflake at Twitter

---

Used for IDs of Tweets (also adopted at twitter and instagram)

Snowflakes are 64 Bits integers

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-4.png)

Snowflake is not a central service, instead it runs app service as a native function

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-5.png)

**Snowflakes are roughly sortable.**

The first 41 bits represent epoch milliseconds, and with each millisecond, the ID moves forward. This allows us to retrieve objects before or after a certain time.

Twitter's pagination is not limit/offset based. Instead, it uses 'since_id' to paginate.

## Snowflake at Discord

---

Same logic as twitter, just epoch = 1st second of 2015 (To get a larger range)

## Snowflake at Sony

---

They call it Sonyflake and their Go-based implementation is open-sourced and can be found on Github
