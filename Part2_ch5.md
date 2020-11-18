### Replication

(Assume that dataset is small enough to be hold on a single machine for this chapter)

three popular algorithms for replicating changes **between nodes: single-leader, multi-leader, and leaderless replication**

Each node that stores a copy of the database is called a replica.

Every write to the database needs to be processed by every replica; otherwise, the replicas would no longer contain the same data. 

- The most common solution for this is called leader-based replication (also known as active/passive or master–slave replication)
    - the writ will always go to leader, and then send data change to followers; followers are read-only;customer can read from any nodes, but only write on leader
 
```
This way is a build-in feature for 

many relational db such as  PostgreSQL (since version 9.0), MySQL, Oracle Data Guard, and SQL Server’s AlwaysOn Availability Groups ;

Non-relational databases, including MongoDB, RethinkDB, and Espresso. 

Finally, leader-based replication is not restricted to only databases: distributed message brokers such as Kafka and RabbitMQ highly available queues also use it. 

Some network filesystems and replicated block devices such as DRBD as well.
```

**Synchronous Versus Asynchronous Replication**
Synchronous: leader waits sending custom confirm(2) until follower has confirmed that it received updates(1)

Asynchronous: leader sends the message but now wait for response from followe

Synchronous good: guarentee always up to date data; bad: can take time for it, especially when some is down,all have to wait

So we have semi-synchronous method, which will have one follower syn, other asyn. If that syn follower is down, one asyn will become syn

asynchronous replication is nevertheless widely used(尽管如此， 还是常用； 坏处：一旦leader 跪了，那么那个write 若还没备份入follower可能就lost了；但不会block其他的write）, especially if there are many followers or if they are geographically distributed. 


**Setting up new followers**

一般可以 先take snapshot， new node request change since that snapshot，一般会有logs，直到caught up

**Handling Node Outages**
    - Follower Failure： Catch up Recovery
    
     from Follower log, find last transaction that was processed before the fault occurred； request all the data changes that occurred during the time when the follower was disconnected. Then caught up.
      
   - LEADER FAILURE: FAILOVER
   
     Handling a failure of the leader is trickier: one of the followers needs to be promoted to be the new leader, clients need to be reconfigured to send their writes to the new leader, and the other followers need to start consuming data changes from the new leader. This process is called *failover（失效备援）*.
     
     1.Determining that the leader has failed(time out usually) 
     
     => 2.Choosing a new leader(election process: The best candidate for leadership is usually the replica with the most up-to-date data changes from the old leader (to minimize any data loss). ) 
     
     => 3. Reconfiguring the system to use the new leader (The system needs to ensure that the old leader becomes a follower, even when it is back and recognizes the new leader.)

    令人忧虑的 ： 比如 旧leader 有一些还没写入的数据，to avoid conflict 我们一般就完全discard了 => 会造成customer data lost（durability problem）；
    
    有时候会很危险，如果 被选出的follower 有落后的version/tools，那么会造成混乱和损失
    
    情形比如出现了两个leader（"split brain" => data is likely to be lost or corrupted.）在设计shut down 其一的时候我们也要小心，有时候可能会shut down both
    
    还有需要在意的， time out 应该设置为多少， 有的时候是因为network/峰值影响 而超过timeout，那么这个时候 再来选举，没有必要且加重了负担！
    


**Implementation of Replication Logs**
   - STATEMENT-BASED REPLICATION
   简单直接，记录每个；但是也有问题： 比如 有些call now（）的会造成replica 值不一样，有些autoincrement的，也可能会造成问题；一些有side effects的 statement，比如trigger或者一些我们不知道的User defined functions
   
   所以这种方法已经少被利用
   
   - WRITE-AHEAD LOG (WAL) SHIPPING
   The log is an *append-only sequence of bytes containing all writes* to the database. We can use the exact same log to build a replica on another node: besides writing the log to disk, the leader also sends it across the network to its followers. 
   
   used in PostgreSQL and Oracle
   
   The main disadvantage is that the log describes the data on a very low level: a WAL contains details of which bytes were changed in which disk blocks. This makes replication closely coupled to the storage engine. If the database changes its storage format from one version to another, it is typically not possible to run different versions of the database software on the leader and the followers. 只记录位置信息，所以和store engine 息息相关，一旦换系统/version 可能会gg
   
   - LOGICAL (ROW-BASED) LOG REPLICATION
   
     所以这种方式，我们不couple storage engine和logs
    
    A logical log for a relational database is usually a sequence of records describing writes to database tables at the granularity of a row:

        - For an inserted row, the log contains the new values of all columns.

        - For a deleted row, the log contains enough information to uniquely identify the row that was deleted. Typically this would be the primary key, but if there is no primary key on the table, the old values of all columns need to be logged.

        - For an updated row, the log contains enough information to uniquely identify the updated row, and the new values of all columns (or at least the new values of all columns that changed).
        
        “change data capture”
   used by MySQL‘s binlog
   
  - TRIGGER-BASED REPLICATION

  上面讲的都是在data system 层面的replication 方法，但是有时候我们需要更机动/customized 的replication 方法，比如handy pick certain/specific table type， 那么就比较需要这个，
  是application layer上的
  
  比如 Oracle GoldenGate， Databus， Bucardo for Postgres
  
  很多时候会有bug的担忧，但比较灵活，所以还是不少情况在用
  
  
  
   

