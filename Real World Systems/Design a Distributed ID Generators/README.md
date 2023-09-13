# Design some Distributed ID Generators

## Snowflake at Twitter

---

Used for IDs of Tweets (also adopted at twitter and instagram)

Snowflakes are 64 Bits integers

![ID distribution demonstration](../../Images/Design%20a%20Distributed%20ID%20Generators/didg-4.png)

Snowflake is not a central service, instead it runs app service as a native function