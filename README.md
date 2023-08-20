# toy-paxos

> A playground to test and learn how Paxos work

Distributed system architecture is an essential framework of many modern day applications, enabling them with scalability, fault tolerence, and high-performance. While these advantages as evident, designing a distributed system presents challenges, Achieving consensus among multiple nodes on a specific value or decision is one such crucial challenge. From synchronizing an online multiplayer game to powering blockchains for cryptocurrencies, consensus  is needed everywhere to guarantee consistency. 

Here we will try to uncover an algorithm that laid the foundation for achieving consensus in distributed systems: Paxos. We will present you a simple implementation of Paxos that helps you script scenarios of systems trying to achieve consensus and see how Paxos make decision at every  stage of protocol to give a deeper understanding of the system.

### Overview

The Notebook here implements a basic Paxos alogrithm and provides a PaxosNode that one can use to simulate various scenarious of the consensus protocol and try to understand how each node makes decision and reach consensus.

#### How to use
First create a PaxosNode with a name and Quorum size 
```
node = PaxosNode("Node", quorum=3)
another_node = PaxosNode("Another_Node", quorum=3)
```

Now initialze a new proposal - A node can re-initialize any time
```
node.init_new_proposal()
```

Request promise from other nodes
```
node.promise_request(another_node)

Output:

PROMISE Request from Node(p: 1) to Another_Node(acc_p: None)
    > [ACC] Promise(1, last_acepted()), Node(promisers: 1, quorum_size: 3, promiser_acc_max: None)

Interpreataion:
* Node sent a promsie request to Another_Node
* Node sent proposal_id p=1
* Annother_node had no preveious accepted proposal acc_p = None
* The proposal was [ACC] Accepted
* Promise was accpted for proposal id 1, another_node had not last_accepted() value in promise response
* Final state of Node is: 1 promiseer, need 2 more to reach quorum of size 3, promiser_acc_max is None as no proposer sent back accepted value otherwise traks max value accpted by any promiser.
```
Request Accept from another node
```
node.accept_request(another_node)

Output:

ACCEPT  Request from Node(p: 1, val: Apple, promiser_acc_max: Mango) to Another_Node(acc_p: 1)
    > [ACC] Accept(1, Mango), Node(acceptors: 3, quorum_size: 3, choice: Mango)

Interpreatation:
* Node sent accept request to Another_Node
* Node's accept request was agains proosal id p_id =1
* Node wants to propose 'Apple'
* Node's promisers have accepted a value 'Mango' i.e promiser_acc_max
* Another ndoe has promised for id acc_p = 1
* Another node [ACC] Accepted the request
* Request was accepted for id 1 and value 'Mango' sent by Node
* Node's final stat is it got 3 accpetors until now, since quorum size is thre it has reached quorum and hence chose the value it requested to acceppt (Mango) as the chosen value.
```
