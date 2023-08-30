# Data Analyst:
    - Dashboards
    - Reporting
    - Excel,SQL
# Data Engineer:
    - ETL
    - Orchestration
    - Python, Java, SQL etc.

#table

| Data Engineers                                | Analytics Engineers		                                                                                        | Data Analysts                                                                                       |
|-----------------------------------------------|--------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Build custom data ingestion integrations      | Provide clean, transformed data ready for analysis                                                           | Deep insights work (ex: Why did churn spike last month? What are the best acquisition channels?)    |
| Manage overall pipeline orchestration         | Apply software engineering practices to analytics code (ex: version control, testing continuous integration) | Work with business users to      understand data requirements    |
| Develop and deploy machine learning endpoints | Maintain data documentation & definitions                                                                    |    Build critical dashboards                                                                                                  |
| Build and maintain the data platform          | Train business users on how to sue a data platform data visualization tools                                                         |          Build critical dashboards                                                                                               | 
| Data warehouse performance optimization       |                                                                                                              | Forecasting                                                                                         | 


**Shards**:
````
Once all column data is encoded, it's written to Google’s distributed file system — Colossus. More decisions 
must be made at this time, the most important one being how to shard the input data. A shard is a unit of 
processing at query time. We don’t want too few shards, because we would like to take advantage of distributed 
processing capabilities of BigQuery, processing a table in parallel using potentially thousands of 
machines — each one reading individual shards. But we also don’t want too many shards, because every unit of 
storage and processing has constant overhead. Future query patterns influence the sharding strategy as well. 
Queries that will read lots of columns may benefit from smaller shards, but queries that read only few might be 
better of with larger shards. Again, BigQuery has a model that evaluates all of the different factors, 
and comes up with an ideal number of shards.
````
**Encoding, Encryption and Replication**:

**External Tables**
````
External tables are a special type of table, where the data resides in a data store that is external to BigQuery, 
such as Cloud Storage. An external table has a table schema, just like a standard table, but the table definition
points to the external data store. In this case, only the table metadata is kept in BigQuery storage. BigQuery does
not charge for external table storage, although the external data store might charge for storage.
````

**Row-oriented databases Vs Column-oriented databases**
````
Row-oriented databases are efficient at looking up individual records. However, they can be less efficient at performing analytical functions across many records, because the system has to read every field when accessing a record.

Column-oriented databases are particularly efficient at scanning individual columns over an entire dataset.

Column-oriented databases are optimized for analytic workloads that aggregate data over a very large number of records. Often, an analytic query only needs to read a few columns from a table. For example, if you want to compute the sum of a column over millions of rows, BigQuery can read that column data without reading every field of every row.

Another advantage of column-oriented databases is that data within a column typically has more redundancy than data
across a row. This characteristic allows for greater data compression by using techniques such as run-length 
encoding, which can improve read performance.
BigQuery does not support foreign keys. This makes BigQuery more suitable for OLAP and data warehouse workloads 
than OLTP workloads. 
````

