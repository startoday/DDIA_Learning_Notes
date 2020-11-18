### Partitioning

For very large datasets, or very high query throughput, **Replication** is not sufficient: we need to break the data up into **partitions**, also known as **sharding**


    
    TERMINOLOGICAL CONFUSION
    
    What we call a partition here is called a shard in MongoDB, Elasticsearch, and SolrCloud; 

    it’s known as a region in HBase, a tablet in Bigtable, a vnode in Cassandra and Riak, and a vBucket in Couchbase. 

    However, partitioning is the most established term, so we’ll stick with that.
    
The main reason for wanting to partition data is scalability. Different partitions can be placed on different nodes in a shared-nothing cluster

1. Partitioning and Replication
    - Partitioning is usually combined with replication even though each record belongs to exactly one partition, it may still be stored on several different nodes for fault tolerance.
    - Each node may be the leader for some partitions and a follower for other partitions.
2. Partitioning of Key-Value Data
    - Our goal with partitioning is to spread the data and the query load evenly across nodes.
    - If the partitioning is unfair, so that some partitions have more data or queries than others, we call it *skewed*. 
    - A partition with disproportionately high load is called a *hot spot*.
    - Avoid uneven => random assign => problem: hard to find, has to query all nodes
    - Partitioning by Key Range
      - The ranges of keys are not necessarily evenly spaced, because your data may not be evenly distributed. In order to distribute the data evenly, the partition boundaries need to adapt to the data.
      - 需要认真选择key range 以防出现hotspot，比如timestamp 可能easy search but one day's partition be a hot spot (**Risk of skew and hot spots)
    - Partitioning by Hash of Key
      - Should use a hash function not that hard, but give you consistent result of hashing (Java's Object.hashCode() is not!!)
      - This technique is good at distributing keys fairly among the partitions. The partition boundaries can be evenly spaced, or they can be chosen pseudorandomly (in which case the technique is sometimes known as *consistent hashing*, but better not to use this word).
      
      
