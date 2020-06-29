# A simple totally ordered broadcast protocol

## Sumukha PK's take


## Sequential reading

* Having the relicated database for processing the Read requests seems like a very useful idea. Many applications can make use of this which helps because generally the read requests are more than the write requests.
* They are solving the idempotency problem by the concept of having a leader/master in the cluster. This is a different take to a heirarchical  cluster than the usual consensus idea.
* They seem to be using some sort of consensus protocol which is tightly bound to the zookeeper service which probably isn't a good idea. (Thought process - not sure at this point)
* They do not assume byzantine faults in the system mainly because they seem to think their idea won't allow such a thing. They seem to report that the systems work fine with given protocol.
* The Zookeeper service uses an in-mem db to write all the relevant data and thus is persistent. I/O bottlenecks are overcome by batched writes.
* Crash-fail is done by having persistent snapshots of the system from time to time and recovering from that snapshot.
* They seem to have a lot of demands from the ZooKeeper service like _low latency_, _bursty high throughput_ and _smooth failure handling_. These depend on the network too.
* They compare their protocol to PAXOS which now makes me think that "broadcast protocol" is somehow meant to maintain consensus in the system.
* This protocol  has a leader election component and the other nodes are called followers.
* The client doesn't necessarily connect to the leader to perform an operation, it can connect to any follower and this balances the load on the servers.
* Zab's protocol has two states; recovery and broadcast. Recovery ends when a leader emerges and the rest of the servers are in sync with the leader. Right after this, it starting Broadcasting messages.
* ZooKeeper service generally has a leader to sync the write requests and maintain replication across its cluster. Here, the same leader is used as the broadcast leader, thus reducing network laterncy - I'm assuming more leaders mean they have to be in sync etc .
* Newly created servers that join to be a part of the Zab service start in recovery mode, sync with leader and then begin with broadcasting - I'm still not sure of the role of every server in the process. Are they just replicas OR do each have to forward the message to some servers so that they share the load. 
* The broadcast protocol: It has a 2 phase commit based operation. The leader gets the request and proposes it to the followers. Once the followers agree, it commits to the followers. (At this point Im not sure whether theyre calling <- THIS operation as the "broadcast" or there's something after this step)
* No aborts makes the aboe process simpler - the idea of cutting down the fat is of the above. Don't use something and complicate it when you won't need it.
* Quoting: _Followers deliver the message when they receive the COMMIT from the leader._ This probably confirms the fact that the leader and followers are a part of a broadcasting, where messages are sent to other servers from this cluster.
* But after the above point we still don't know what happens when the follower fails. Because, if the follower was assigned to pass on the message to some set of servers like the other nodes in the Zab cluster, who sends to those nodes when there's a follower failure?
* Leader election process in the recovery phase seems similar to raft. There are timer components and error/indecisiveness based restarts until a leader is reached.
* There are 2 implementations of the leader election for unknown reasons at this point. They claim the fastest one is in the order of 100ms.
* Recovery can be hard because there might be requests that are mid-way in their processing pipeline. Thus, delivered messages aren't forgotten and skipped messages ARE let go.
* They still talk about maintaining consensus in the cluster and thats the most important thing to maintain the operations running and clean.
* Failure of the leader before the COMMIT but after the acceptance and proposal from client and to the followers respectively is a good example of an inconsistency.
* There is a talk of "skipped messages" which seem to be those which have a proposal generated for them but the leader fails right after.
* Remembering _delivered messages_ is done by tweaking the leader election protcol. The newly elected leader will be guaranteed that it'll have all the older committed messages and before proposing new messages, this new leader will ensure all messages in its log are proposed AND committed.
* Quoting: _The leader ensures that its followers have seen all the proposals and have deliveredall that have been committed._ This further confirms the cloud over what the broadcasting actually meant.
* Above is achieved by adding proposals to all the messages it hasn't seen and then committing them too so that it's delivered to all the necessary places.
* Skipping messages are also easy imo because since it was never committed, the client knows that it wasnt broadcasted. These can be ignored now and then the client can contact back if needed.
* They've linked the concept of having a 64bit counting mechanism to the ease of skipping messages. This is because, if a random server that had failed before is restarted, it cannot be the leader as it has messages from the previously delivered epoch that has not been delivered because other servers have a higher epoch. (There is a restriction that new leaders must have the highest epoch where the epoch is the first 32bit of the 64bit counting mechanism.) Now this new node connects as a follower and the then leader will ask it to truncate its older messages thus _easily_ skipping messages.

## Overall idea

* The whole idea of the paper is to build a set of systems that broadcast commands that are sent by client to a distributed system of servers.
* There didn't seem to be any new thing I learnt from this paper as it seems to be quite old -2008. Not quite old but ...
* The paper seems a little hard to understand because there is no clear mention about the "broadcasting" late in the paper.
* They seem to have merged the idea of a consensus module into the broadcasting requirements and that doesn't seem like a good idea. Maybe they can function better as separate modules.
* This was an old idea, 6 years before Raft was found so seems like a time where consensus was still being thought of and never hitting it right.
* Maybe building such a system is interesting due to the intricacies involved but this paper did nothing much to inspire me.


