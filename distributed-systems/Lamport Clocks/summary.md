# Lamport Logical Clocks

The motivation behind this paper is to come up with an algorithm that can establish a 'happened before' ordering of events in a distributed system. In a distributed system, there is no way that the clocks of all the entities involved can be exactly synchronised. In the event of a crash, if certain transactions or events need to be *replayed*, then it is impossible to know which event *happened before* the other. 

In this paper, Leslie Lamport has proposed a simple algorithm to establish a logical clock between entities that interact with each other. If entities do not interact with each other, then there is no need for them to share a logical clock. 

The algorithm is as follows: 
1. Each entity has its own counter. This counter is incremented between any two events that occur in this entity (process), therefore no two events in the same process share the same counter value. 
2. When a message is sent from one process to the other, the sender marks a timestamp of its counter on the message. On receiving this message, the receiver updates its counter only if the timestamp of the message is greater than its current counter value. 

The same algorith can be expressed as code segments:

On the sender's side:
```
time = time + 1 
// event occurs

time = time + 1
timestamp = time
message = NewMessage(contents, timestamp)
send(message)
```

On the receiver's side:
```
contents, timestamp = recieve()
time = max(timestamp, time) + 1
```

With this logical clock, if there is a crash and these events need to be replayed, it is relatively simple as there is an inherent ordering establlished due to the logical clock shared by the processes.