# Database Indexes

Indexes are database structures that significantly enhance the efficiency of data retrieval operations, such as SELECT queries, by facilitating faster lookup of specific rows or records. However, they come with trade-offs:

**_Read Performance Improvement_**: Indexes indeed make reads faster. By creating an index on one or more columns, the database can quickly locate the relevant rows without scanning the entire table. This results in reduced query execution times, making it ideal for SELECT operations, especially when dealing with large datasets.
