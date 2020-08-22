# Brewer's Conjecture and the Feasibility of Consistent, Availabile, Partition-Tolerant Web Services 

## Running points:
* The paper seems to prove a very fundamental idea in DS - the CAP theorem.
* Consistency - Every read operation that happens after a write must return the updated value.
* Availability - Every request made to a non-failing node must result in a response.
* Partition Tolerance - Partitions if when they occur in the network must not lead to inconsistent data in the system.
* _No set of failures than a TOTAL system failure is allowed to cause the system to respond incorrectly_.
* It has a couple of theorems that prove with and without existance of "lost messages" in the system that having consistency and availability is not possible WHEN THERE ARE PARTITIONS IN A SYSTEM.
* Achieving Atomicity and P.tolerance:  
  Since the system need not be available, there can be a centralised system that maintains everything, thus satisfying all criteria.
* Achieving Atomicity and Availability:  
  Since the system can have no partitions, the system can easily achieve the above criteria.
* Achieving Availability and P.tolerance:  
  Since the system need not worry abount consistency, it can always return the initial value of the data which is the "wrong value".
* They say in the first theorem that in a completely async system, there is NO way to ensure Avail and consistency but with inclusion of slight sync it might be possible.
* The second theorem proves that, even if theres a sync, its not ALWAYS guaranteed that Avail and consistency can be acheived.
* There cant be a corollary based on the second theorem like corollary 1 because, if all messages ARE delivered, the system can achieve the desired criteria because of the sync that exists between them.
* A _Delayed-t_ consistency idea is put forward based on having a partial order in the execution and the operations behaving the said partial order. This consistency is also maintained when the operatios are _atomic_ and _weakly consistent_.
* A modified centralised algorithm is proposed which is _Delayed-t_ consistent.
* Proof of the above is based on the fact that the centralised node maintains a partial order of the requests and thus maintains atomicity.
* Conclude with telling that CAP together is not possible.
