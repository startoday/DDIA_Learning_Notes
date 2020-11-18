### Transactions

A transaction is a way for an application to group several reads and writes together into a logical unit.

1. The Slippery Concept of a Transaction
  - Acid: Atomicity, Consistency, Isolation, and Durability; Not really standard and Useful in usage; Oppsite: BASE, Basically Available, Soft state, and Eventual consistency
    - Atomicity: In general, atomic refers to something that cannot be broken down into smaller parts. 
                 The ability to abort a transaction on error and have all writes from that transaction discarded is the defining feature of ACID atomicity. 
                 
