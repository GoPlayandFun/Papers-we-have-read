# Paxos Made Simple

## Bird's Eye View

- **Category** - The paper essentially explains a concept. Paxos is known for being complicated. This paper aims to show that it is simple.
- **Context** - It is a distributed consensus algorithm. Falls under the same bracket of Raft, Bitcoin etc. More of a Theoretical CS / Algorithms paper than a Systems one.
- **Correctness** - Not sure
- **Contribution** - Making the algorithm more understandable. Not sure what else.
- **Clarity** - After a first glance, paper seems to be well written.


## The Problem
Say a bunch of processes propose values. The consensus algorithm has to make sure that one of these values is chosen. Additionally, 

- Only a proposed value can be chosen
- Only a single value is chosen

Different roles in a consensus algorithm - 
- Proposers
- Acceptors
- Learners

In an implementation, a single process can have more than one of these roles.

Asynchronous, non-Byzantine model: 
1. Messages can be lost and duplicated but not corrupted
2. These agents operate at arbitrary speed, they can fail and restart. There must be a way for at least one of these agents to remember a value even if they have failed and restarted.

## Choosing a Value
Easiest way would be to have a single acceptor that accepts the first proposal that it receives. However, if the acceptor crashes/fails, there is a problem. 

Hence, instead we have multiple acceptors and the chosen value is one that is accepted by any majority of the acceptors. 

An acceptor accepts the first value it receives. However, each acceptor could receive a different value initially. Hence, consensus may never be reached if each acceptor accepted only a single value. Therefore, an acceptor can accept more than one proposal. A proposal consists of a proposed value and a unique number. The number is used to keep track of all the proposals accepted by an acceptor. Multiple proposals can be 'chosen', but they must have the same value.

If a proposal with value 'v' is chosen, then any higher numbered proposal proposed by a proposer must have value 'v'. Therefore, a proposer who wishes to issue a proposal with number 'n', must learn the highest numbered value accepted by some majority of acceptors. It can see what has already been accepted. But what proposals numbeered less than n that might be accepted after it has sent its proposal numbered n?

### Algorithm of a Proposer

- **Prepare Request** - Choose a proposal number n and send a prepare request to a subset of all acceptors with a promise to not accept any propsoal numbered less than n. Also, to send back the highest numbered value accepted, if any.

- **Accept Request** - If responses are received, then to any subset of acceptors (can be diff from the set prepare req was sent to), issue a proposal numbered n, either with the same value v as the highest numbered accepted value received, or a new value if acceptors have not got any proposals.


> What is a Byzantine failure? What is a non-Byzantine failure?

## Thoughts
Really well written and intuitive paper to read. I haven't read the original Paxos paper, so not sure how difficult the algorithm in itself seems. I really like the fact that the paper addressed problems such as when two proposers constantly keep proposing values with higher proposal numbers and the potential network traffic as a result of messages needing to be sent from all acceptors to all learners. These are questions that naturally crept into my mind. I only skimmed through the final section of the paper, but this was a real treat to read!