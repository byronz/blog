---
title: "Mongo"
date: 2018-02-22T15:49:36-05:00
tags: [nosql, document, mongo, db, cluster, shard]
---

## Replication Set

A replica set in MongoDB is a group of mongod processes that maintain the same data set

![replication](https://docs.mongodb.com/manual/_images/replica-set-read-write-operations-primary.bakedsvg.svg)

A replica set can have **only one primary** capable of confirming writes with { w: "majority" } write concern

election new leader takes about 10-30s before, after v3.2 ~2s

![arbiter](https://docs.mongodb.com/manual/_images/replica-set-primary-with-secondary-and-arbiter.bakedsvg.svg)

## Shared Cluster
https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/

![cluster](https://docs.mongodb.com/manual/_images/sharded-cluster-production-architecture.bakedsvg.svg)

- shard: Each shard contains a subset of the sharded data. Each shard can be deployed as a replica set.
- mongos: The mongos acts as a query router, providing an interface between client applications and the sharded cluster.
- config servers: Config servers store metadata and configuration settings for the cluster. As of MongoDB 3.4, config servers must be deployed as a replica set (CSRS).

**Shard key**


## TS Model

*typical document design*
```
{
  timestamp: ISODate("2013-10-10T23:06:37.000Z"),
  type: ”memory_used”,
  value: 1000000
},
{
  timestamp: ISODate("2013-10-10T23:06:38.000Z"),
  type: ”memory_used”,
  value: 15000000
}
```

*with ts-oriented*
```
{
  timestamp_minute: ISODate("2013-10-10T23:06:00.000Z"),
  type: “memory_used”,
  values: {
    0: 999999,
    …
    37: 1000000,
    38: 1500000,
    … 
    59: 2000000
  }
}
```

MongoDB has in-place update mechanism or built-in operator `{ $inc: { pageviews: 1 } }`


## MMS

https://www.mongodb.com/post/10764757533/announcing-mongodb-monitoring-service-mms

## References

* https://www.mongodb.com/blog/post/schema-design-for-time-series-data-in-mongodb
* https://docs.mongodb.com/manual/sharding/
* https://docs.mongodb.com/manual/replication/

## Comparision 

https://scalegrid.io/blog/cassandra-vs-mongodb/

https://docs.mongodb.com/manual/core/replica-set-write-concern/

https://stackoverflow.com/questions/2892729/mongodb-vs-cassandra

    Both databases perform well on reads where the hot data set fits in memory. Both also emphasize join-less data models (and encourage denormalization instead), and both provide indexes on documents or rows, although MongoDB's indexes are currently more flexible.

    Cassandra's storage engine provides constant-time writes no matter how big your data set grows. Writes are more problematic in MongoDB, partly because of the b-tree based storage engine, but more because of the per database write lock.

    For analytics, MongoDB provides a custom map/reduce implementation; Cassandra provides native Hadoop support, including for Hive (a SQL data warehouse built on Hadoop map/reduce) and Pig (a Hadoop-specific analysis language that many think is a better fit for map/reduce workloads than SQL).

    Not worried about "massive" scalability

    If you're looking at a single server, MongoDB is probably a better fit. For those more concerned about scaling, Cassandra's no-single-point-of-failure architecture will be easier to set up and more reliable. (MongoDB's global write lock tends to become more painful, too.) Cassandra also gives a lot more control over how your replication works, including support for multiple data centers.

    More concerned about simple setup, maintenance and code

    Both are trivial to set up, with reasonable out-of-the-box defaults for a single server. Cassandra is simpler to set up in a multi-server configuration since there are no special-role nodes to worry about; here is a screencast demonstrating setting up a 4-node Cassandra cluster in two minutes.

    If you're presently using JSON blobs, MongoDB is an insanely good match for your use case, given that it uses BSON to store the data. You'll be able to have richer and more queryable data than you would in your present database. This would be the most significant win for Mongo.