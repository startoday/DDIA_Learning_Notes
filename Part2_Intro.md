Reasons For distribute a database 

  - Scalability
  
    When your need is more than a single machine can handle
    
  - Fault tolerance
  
    When one machine is fail, we need multiple machine
  
 - Latency
 
    When there are more severs on the world, it can reduce the latency custom needs to wait
  
 
Scaling to Higher Load
    
  -  **Shared-Nothing Architectures**

    In this approach, each machine or virtual machine running the database software is called a node. 

    Each node uses its CPUs, RAM, and disks independently. 

    Any coordination between nodes is done at the software level, using a conventional network.

  - **Replication VS Partitioning**

    *Replication*
    
    Keeping a copy of the same data on several different nodes, potentially in different locations. 

    Replication provides redundancy: if some nodes are unavailable, the data can still be served from the remaining nodes. 

    Replication can also help improve performance.
    
   *Partitioning*
   
   Splitting a big database into smaller subsets called partitions so that different partitions can be assigned to different nodes (also known as sharding).
   
   
