# Paxos Made Simple

* I've always heard from the Raft paper and other resources that the paxos algorithm was very complex and hard to understand. This paper aims to simplify it.
* They say that the original paxos paper was "Greek" and was one of the factors leading it to being complex to understand.
* They also say that and I quote - "The paxos algorithm when presented in plain english, is very simple".
* They say that the core of the algorithm is based on the "synod" algorithm of Leslie Lamport himself.
* The description of a consensus problem is a bit different here:
   * Assuming a collection of processes can propose values, a consensus alg. ensures that only a single of the values is chosen.
   * If there were no values proposed then no value should be chosen.
   * If a value is chosen, then the processes must be able to learn the value.
   * Some safety conditions must be ensured to make sure that the alg. holds good; like only a value that has been proposed has to be chose and nothing else, always a single value has to be chosen and a process never learns that a value has been chosen unless it actually has been.
* There exists 3 roles in the consensus algorithm - _proposers_, _acceptors_ and _learners_. One 
* They are assuming a non-ideal system (non-Byzantine model) where messages can take long times to be delivered, can be lost, duplicated but never corrupted.
* Choosing a value:
  * A value is said to be chosen when a majority of acceptors have chosen the value sent by the proposer.
  * With a condition of - "The acceptor must accept the first value that was proposed to it", we can't ensure that there is a majority of any one said value in case of multiple proposers and multiple acceptors.
  * Thus, each value is assigned a proposal number and when a majority is reached for the value, then the value is said to be chosen.
  * We can allow multiple proposals to be chosen but all these must have the same proposal value.
  * With a condition of - "If a proposal with value _v_, is chosen, then every proposal with number greater than it must have the same value" they ensure that only a single value proposal is chosen. 
  * _P2(b), P2(a) and P2_ imply the next in that order because - for a proposal to be accepted, it has to be first proposed and for a proposal to be chosen it has to be first accepted.
  * _P2(c)_ says that if there a proposal _n_ with value _v_ that has already been accepted then (a) There is no other acceptance with proposal number less than _n_ or (b) _v_ is the value of the highest proposal number _n_ that has been accepted.
  * All above leads to the following _Proposer's algorithm_:
    * Prepare Request: A proposer sends a proposal request with it proposal number _n_ to some set of acceptors and asks them to respond with (a) a promise never to accept a proposal with number less than _n_ and (b) If it has already accepted something, return the highest proposal number less than _n_.
    * Accept Request: If the proposer receives a majority of the "requested response" from the majority of the acceptors, it can issue a proposal number _n_ and value _v_, where _v_ is the highest numbered proposal among the responses or any value if there were no responders for the Prepare Request.
  * Acceptors receive 2 types of requests, prepare and acceptance requests. The acceptor can safely choose not to respond to these requests. The acceptor can also safely respond to ALL Prepare Requests. It can respond to an Accept Request, by an acceptance, iff it has promised not to.
  * _P1(a)_ says that an acceptor can accept a a proposal numbered _n_ iff has not responded to a _prepare_ request with a number greater than _n_.
* Learning a chosen value:
  * The chose values are propogated from the acceptor right after the value is accepted to a set of distinguished learners who then propogate to the rest of the learners.
  * Having one distinguished learner might lead to the process stopping if the d.learner fails thus having more d.learners increases reliability.
  * If the key points in the above statements fail, then the learner can learn from the proposer by using the algorithms above. (This has the accepted value)
* Progress:
  * Multiple proposers can keep proposing values and still not lead to a majority. Thus some sort of algorithm must govern the progress.
  * A distinguished proposer must be selected as the one trying to issue the proposals. Now, if it follows the above protocol, the process will succeed.
  * The distinguished proposer is selected by randomness or using time outs (real-time).
* Implementation:
  * The paxos algorithm assumes a network of processes. Each of these processes assume the role of proposer, acceptor and a learner.
  * The algorithm chooses a leader and this process becomes the distiguished proposer and the distinguished learner.
  * The proposal numbers are always chosen from a disjoint set of numbers when multiple proposers exist.
  
  
  
## Takeaways

The implementation section describes how the _state machine_ of a single node must behave. Though I har heard this concept of a state machine in Raft too, I never implemented it this way. This "state machine way" is something where external stimulus sends request to the state machine and it changes states, deterministically, based on the request.
On deeper thought, the implementation of the state machine would probably more or less end up the way I implemented raft for `lbadd` (now `xdb`) - wait for external client calls and execute different functions based on it. 
  
  
  
