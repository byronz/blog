---
title: "Riak"
date: 2018-02-22T16:06:02-05:00

---

## Riak KV

Note: *I think the official documentation quality is very poor, a lot of links are 404 and not updated*

### Query option

- Riak search via Apache Solr 
- Secondary Index
- Map Reduce



## Riak TS


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

### Cluster

![admin](/img/riak.png)

```yml
version: "2"
services:
  coordinator:
    image: basho/riak-ts
    ports:
      - "8087:8087"
      - "8098:8098"
    environment:
      - CLUSTER_NAME=riakts
    labels:
      - "com.basho.riak.cluster.name=riakts"
    volumes:
      - schemas:/etc/riak/schemas
  member:
    image: basho/riak-ts
    ports:
      - "8087"
      - "8098"
    labels:
      - "com.basho.riak.cluster.name=riakts"
    links:
      - coordinator
    depends_on:
      - coordinator
    environment:
      - CLUSTER_NAME=riakts
      - COORDINATOR_NODE=coordinator

volumes:
  schemas:
    external: false
```

`docker-compose scale member=4`
## References

* https://hub.docker.com/r/basho/riak-ts/