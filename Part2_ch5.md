### Replication

(Assume that dataset is small enough to be hold on a single machine for this chapter)

three popular algorithms for replicating changes **between nodes: single-leader, multi-leader, and leaderless replication**

Each node that stores a copy of the database is called a replica.

Every write to the database needs to be processed by every replica; otherwise, the replicas would no longer contain the same data. 

- The most common solution for this is called leader-based replication (also known as active/passive or master–slave replication)
    - the writ will always go to leader, and then send data change to followers; followers are read-only;customer can read from any nodes, but only write on leader
 
This way is a build-in feature for many relational db such as  PostgreSQL (since version 9.0), MySQL, Oracle Data Guard, and SQL Server’s AlwaysOn Availability Groups ;Non-relational databases, including MongoDB, RethinkDB, and Espresso. Finally, leader-based replication is not restricted to only databases: distributed message brokers such as Kafka and RabbitMQ highly available queues also use it. Some network filesystems and replicated block devices such as DRBD as well.

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
      from Follower log, find last transaction that was processed before the fault occurred； request all the data changes that occurred during the time when the follower was disconnected. Then caught up

