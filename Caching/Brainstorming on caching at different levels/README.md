# Brainstorming on caching at different levels

1. In main memory of API Servers
   1. Limited to one api server
   2. If API server crashes, the data would be lost
   3. Inconsistency (If you cache the data in one server, when the client hits the same api endpoint but in different server, then that server might have to make call to the database, as the cache is not centralised)
   4. Burden on actual server (Due to usage of main memory)
2. Database views (**Materialised**)
   1. Stale data (Although you can use triggers to update these [materialised views](https://docs.snowflake.com/en/user-guide/views-materialized))
3. Browser (**Local Storage**)
4. Personalised Recommendation, storing the recommendation data on the browser itself, and periodically fetch the recommendation from the server (To refresh the Data). eg: Storing 50 Recommendations, and when user is trying to view all the recommendation, we can simply display them some amount of the recommendation data we have (let’s say 10), that would provide **a smooth user experience**
5. CDN (Caching the **response**). eg: Caching search results of popular terms
6. Disk of API Server (**IT WORKS VERY WELL!**) (**Stateful**)
   1. Why not leverage the disk that your api server has?
   2. A guy at Goldman Sachs made this optimisation
7. Cache data in load balancers (**Like responses**)
8. Data can also be cached at the ISP level, as Netflix did with their media data, in order to provide low latency access to shows for users
