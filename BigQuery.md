##Shards:
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

