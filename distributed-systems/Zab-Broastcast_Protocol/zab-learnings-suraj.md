# Zab - Totally Ordered Broadcast Protocol

## Learnings
- Essentially the motivation for this algorithm emerges from the ZooKeeper service at Yahoo and hence most of the requirements of this protocol stem from the needs of ZooKeeper.
- This paper describes the requirements made for Zab, how it is used and how it works.
- The paper mentions that read throughput of a system can be increased by adding replicas of servers, however, the write throughput is dependant on the broadcast protocol. 
- The protocol in itself is a pretty simple variation of a two phase commit. It is 'easier' since the protocol does not have to deal with aborts.
- Didn't fully understand what the authors meant by idempotency in this context, but in one section of the paper it was mentioned that since transactions are idempotent, if a server receies duplicates, it will matter. 
- The paper says that _the leader does not have to send the COMMIT if the followers broadcast the ACKs to each other_. Implication of this is the servers will have to be in a mesh topology rather than a star topology. I didn't quite understand what this quoted text from the paper actually meant.

## Thoughts
- From what I've understood, essentially all the servers in this system must maintain the same values. Doesn't a consensus algorithm achieved this. They mentioned some shortcomings of Paxos but would Raft serve our purpose as well?
- Paper was not reader friendly. It assumes you are well versed with ZooKeeper and also I felt that a lot of what was talked about in the initial section became clear only after reading Section 4.
- Protocol in itself seems simple on both a conceptual and implementational level which is exactly what they have reported.