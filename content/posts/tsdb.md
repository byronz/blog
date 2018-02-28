---
title: "NOSQL DB"
date: 2018-02-09T18:59:44-05:00
---

# Cassandra

Cassandra’s data model consists of keyspaces, column families, keys, and columns.

## Data Model

```Map<RowKey, SortedMap<ColumnKey, ColumnValue>>```


![row](https://shermandigital.com/img/blog/cassandra-partition.png)

![example](https://pandaforme.gitbooks.io/introduction-to-cassandra/content/Screen%20Shot%202016-02-24%20at%2011.46.09.png)

```SQL
CREATE TABLE crossfit_gyms_by_city (  
 country_code text,  
 state_province text,  
 city text,  
 gym_name text,  
 opening_date timestamp,  
 PRIMARY KEY ((country_code, state_province, city), opening_date, gym_name)  
) WITH CLUSTERING ORDER BY ( opening_data ASC, gym_name ASC );
```


## Partitioner

A partitioner determines how data is distributed across the nodes in the cluster (including replicas). 

- Murmur3Partitioner (default): uniformly distributes data across the cluster based on MurmurHash hash values.
- RandomPartitioner: uniformly distributes data across the cluster based on MD5 hash values.
- ByteOrderedPartitioner: keeps an ordered distribution of data lexically by key bytes


![hash](https://pandaforme.gitbooks.io/introduction-to-cassandra/content/Screen%20Shot%202016-02-23%20at%2015.24.40.png)

## R/W

![Read](https://academy.datastax.com/sites/default/files/read-path.png)

To satisfy a read, Cassandra must combine results from the active memtable and potentially multiple SSTables. Cassandra processes data at several stages on the read path to discover where the data is stored, starting with the data in the memtable and finishing with SSTables:

- Check the memtable
- Check row cache, if enabled
- Checks Bloom filter
- Checks partition key cache, if enabled 
- Goes directly to the compression offset map if a partition key is found in the partition key cache, or checks the partition summary if not If the partition summary is checked, then the partition index is accessed
- Locates the data on disk using the compression offset map
- Fetches the data from the SSTable on disk

![write](https://academy.datastax.com/sites/default/files/write-path.png)

- Logging data in the commit log
- Writing data to the memtable
- Flushing data from the memtable
- Storing data on disk in SSTables

## Limits

- No join or subquery support for aggregation. According to Cassandra’s documentation, this is by design, encouraging denormalization of data into partitions that can be queried efficiently from a single node, rather than gathering data from across the entire cluster.
- Ordering is set at table creation time on a per-partition basis. This avoids clients attempting to sort billions of rows at run time.
- All data for a single partition must fit on disk in a single node in the cluster.
- It’s recommended to keep the number of rows within a partition below 100,000 items and the disk size under 100 MB.
- A single column value is limited to 2 GB (1 MB is recommended).

## Records

cluster logs and folder structure
[![asciicast](https://asciinema.org/a/nemZPToq1rZNKXt57LbhqBidb.png)](https://asciinema.org/a/nemZPToq1rZNKXt57LbhqBidb)

connect to cassandra using python driver
[![asciicast](https://asciinema.org/a/QSrgG1SQFcRFhrCUowag7lENt.png)](https://asciinema.org/a/QSrgG1SQFcRFhrCUowag7lENt)


## Reference

* https://shermandigital.com/blog/designing-a-cassandra-data-model/
* http://abiasforaction.net/apache-cassandra-cluster-docker/
* http://abiasforaction.net/a-practical-introduction-to-cassandra-query-language/
* https://hub.docker.com/_/cassandra/
* https://academy.datastax.com/resources/getting-started-time-series-data-modeling
* https://academy.datastax.com/resources/getting-started-time-series-data-modeling
* https://pandaforme.gitbooks.io/introduction-to-cassandra/content/understand_the_cassandra_data_model.html
 


# Riak TS

```SQL
CREATE TABLE GeoCheckin
(
   region      VARCHAR   NOT NULL,                   -
   state       VARCHAR   NOT NULL,                    |
   time        TIMESTAMP NOT NULL,                    |  --> Column Definitions
   weather     VARCHAR   NOT NULL,                    |
   temperature DOUBLE,                               _
   PRIMARY KEY (
     (region, state, QUANTUM(time, 15, 'm')),        <-- Partition Key => group data that will be queried together in the same physical part of the cluster
     region, state, time                             <-- Local Key =>  
   )
)
```
    Note:  Only one quantum function may be specified and it must be the last element of the partition key.



# MongoDB

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

## References

* https://www.mongodb.com/blog/post/schema-design-for-time-series-data-in-mongodb
