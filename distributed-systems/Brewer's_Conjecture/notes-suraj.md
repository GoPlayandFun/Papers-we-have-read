# Brewer's Conjecture

Abstract - Proving that achieving consistency, availability and partition tolerance in an asynchronous model is impossible.

## What the reader will be able to take away from reading this summary

- Clear understanding of what is meant by C, A and P.
- Understanding what asychronous and synchronous network models are.
- The logic behind the proof presented by the authors

## Summary
It is important for a webservice to provide C, A and P. 

### Consistency
Most webservices provide strongly consistent data. Consistency is essentially ensured by using ACID databases.

**A**tomic - Transactions either commit or fail in their entirity
**C**onsistency - A committed transaction is visible to all future transactions
**I**solated - Uncommited transactions are isolated from each other
**D**urable - A committed transaction is permanent.

Atomic consistency - There must exist a total order on all operations such that each it looks like each operation were completed at a particular instant.

### Availability
Every request should succeed and receive a response.

Some defintions of availability may require requests to be serviced within some max limit. Similar to how SLAs are in data centers.


### Partition Tolerance (Fault Tolerance)
If nodes in the distributed system or communication links fail, the service should continue to work as expected. A particular case of failure is a network partioning into several components.
![Network Partition](https://blog.yugabyte.com/wp-content/uploads/2019/05/How-Does-YugaByte-DB-Handle-Network-Partitions-and-Failover-blogpreview.png)


As depicted in the image above, a network partition may be modelled as an arbitrary loss of lots of packets between two nodes in a network. Because of the C and A guarantees, even in the event of a network partition, an atomic (complete) response must be generated when servicing a request. This is regardless of whether packets are dropped between different nodes. Unless there is a complete network failure, the web service should not behave incorrectly.

### Asynchronous Network Model
There is no clock! Decisions must be made on messages received and local computation. [side note: Maybe a method to make these decisions could be Lamport Timestamps?]

### Proof that all three consistency, atomicity and partition tolerance cannot be guaranteed in asynchronous network model
Assume a network contains at least two nodes. Then the network can be divided in two disjoint, non-empty sets {G1, G2}. Let V0 be the state of an atomic object. Let a write operation not equal to V0 occur in G1. Assume that no packets can be sent between G1 and G2. Because of the availability property, this request will be satisfied. Next, assume a read operation is sent to G2 and again no packets can be shared between G1 and G2. Again, because of the availability property, this request will be satisfied but will return V0, and not the modified value. Hence, this contradicts the atomic consistency property.













