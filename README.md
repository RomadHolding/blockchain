These are the preliminatry design notes and considerations for ROMAD blockchain.

# 1. Problems to be solved
There are 3 main problems [1] for the distributed ledger technology (DLT) developers:
- to ensure the ledger speed and scalability;
- to ensue the ledger security;
- to make the ledger as decentralized as possible in terms of the functioning and control.

## 1.1. Speed
The transactions speed (measured in transactions per second or tx/s) is the main DLT problem. Why this is important:
1. Micro transactions. There will be many of them and they need to be done fast.
2. The DLT-based payment system with the many users. Visa claimed in 2011 of being able to achieve 24К tx/s [2]. This value is the de facto bandwith golden standard for the payment systems since then. For example:
 - Ripple is able to achieve 1500 tx/s [3].
 - IOTA. When operating at 1000 tx/s IOTA requires 1.6Mb/s of its nodes' bandwith. ***The IOTA live journal shows ~14.0 tx/s at the time of writing*** [4].

The researchers are operating with the following DLT speed charateristics:
 1. The transactions per second ratio. This is by no means is an exhaustive value. It is possible for the ledger to suspend the transactions for an hour, then it processes 1M of them and the average value will be 277 tx/s.
 2. The transaction latency. There is a time when the transaction is issued and a time when the transaction is initially confirmed. The time difference between these two events is the latency. This tends to be a more objective measurement metric, however it is not exhaustive as well.
 3. The finality is when the transaction is closed, i.e. there are no means to cancel or rollback it. In fact one can only trust the transfer performed by, e.g. a smart-contract, when the transaction is finalized.

The most balanced option about the DLT speed charateristics is in the article of Andrei Grigorean at Hackernoon [5]:
 1. "For small payments, merchants would probably accept a payment the moment the transaction is initially confirmed, provided that they have a reasonably high confidence the payment will be accepted eventually."
 2. However: "for large money transfers, the receiver of the funds would probably want to wait for the transaction to become irreversible. Or at least, enough time should pass until a probabilistic finality is reached."
 3. And here we see one of the biggest DLT problems: "...In the financial industry, institutions need to know, preferably as quickly as possible, whether they truly own certain assets. If a public distributed ledger technology (DLT) is used to store the ownership information, the institutions also need to be sure it will not be possible to revert a certain transaction, making them lose the ownership rights..."

## 1.2. Security
The DLT security actually assumes the system is able to function correctly when some nodes are performing incorrectly. The incorrectness can be a consequence of:
- the simple hardware or software errors, i.e the node is faulty;
- the malicious intents, i.e. the node behaves maliciously;
- the load factor, i.e. the node is overloaded either because of the software design mistakes or the malicious intents;
- the DLT network segmentation, i.e. some nodes cannot see the other ones.

The correctness of the DLT functioning is the subject of the numerous debates and researches. ROMAD uses the defenitions from [6] "it suffices to build a totally globally-consistent, totally- ordered, append-only transaction log"
There are the following consequences from the definition:
- it prevents the double-spend attack;
- it prevents the transactions re-ordering;
- it prevents the ability of the certain nodes to lie about the transactions;

## 1.3. Decentralization and openness
Decentralization is the DLT ability that assumes the decision is made by the majority of the nodes in the network. Ideally all the nodes should vote. This allows for DLT to function without the need to trust to the individual nodes. Instead the DLT trusts to the majority only.

The analytics is presented mainly in [7] - [10].

|          | Public                                                      | Private/Federated              |
| -------- | ----------------------------------------------------------- | ------------------------------ |
| Access   | Open read/write                                             | Permissioned read and/or write |
| Speed    | Slower                                                      | Faster                         |
| Security | Proof of Work / Proof of Stake / Other consensus Mechanisms | Pre-approved participants      |
| Identity | Anonymous / Pseudonymous                                    | Know identities                |
| Asset    | Native Asset                                                | Any Asset                      |

These are the edge cases. In general it is hard to distinguish between Public/Private and Permissioned/Permissionless. There are private ledgers that use the consensus and there are public ledgers (e.g. NEO) that use the pre-approved supernodes for the consensus.

## 1.4. ROMAD DLT requirements

  1. **The more tx/s, the better. Low latency.** tx/s is important in the commercial applications. ROMAD team does not forsee the significant amounts within the transactions in the near future so that the latency is not that important. However we want to keep it as low as possible.
  2. ROMAD DLT is **resilint to the bad nodes**. These include the malicious ones as well.
  3. **Bit rate on writes**. ROMAD's ledger is to contain the malware data on the Stage I, so the write speed is very important.
  4. **Disk space**. The less, the better.
  5. ROMAD's ledger is the **public and open** one.

# 2. DLT types
As shown in [11], the Distributed Ledger was introduced in late 1990th and was based on the Mutual distributed ledger (MDL) data structure. In fact the same transaction was stored in multiple databases. DL has raised significant attention in 2008 as the cryptocurrencies foundation. The first public DLs were based on the blockchain structure. As the field became more matured, the alternatives were proposed. DAG (Directed Acyclic Graph) was introduced in 2016-2017. Tempo Ledger was introduced in 2018. To sum it up:
- Blockchain;
- DAG;
- Tempo Ledger.

Blockchain-based DLT properties are:
1. The ordered linked list that consists of the blocks. Merkle tree.
2. The block size problem
3. There are orphaned blocks [26]

DAG-based DLT properties are:
1. The directed acyclic graph is at the foundation. The verticies are the transactions, the edges are connections between the transactions.
2. Fast initial confirmation [27], [28].
3. Better scalability, no block size problem [29].
4. No orphaned blocks
5. No delays when sending the transactions to the network.
6. Transactions finality (Some DLTs do not have it, like IOTA)

Some comparisons are present in [11]:

![Ledger Comparsion](./LedgerCompasion_01.jpg)

Tempo Ledger is currently not very well studied, so ROMAD is not going to use it. ROMAD blockchain is based on DAG, as it is the fastest and most compact data structure currently proposed for DL implementation.

## 3. Consensuses
### 3.1. Consensus protocols families
There are many known consensus protocols that can be used in DLT implementation [11].

One of the possible classifications goes below.

* Proof based consensus – a single node that uses some valuable resource to ‘justify’ the decision made in fact makes the decision. If the decision does not correspond to the rules, it is ignored and the resource is wasted.  The computation capacity is used for the Proof-of-Work. The computation time is used for the Proof-of-Elapsed Time. The deposits are used for deposit-based consensuses.  Such systems have various checks to prove that the resource was spent.
* Despite the common belief, we believe the Proof-of-Reputation, the Proof-of-Importance and the Proof-of-Stake (without deposit) to be a separate group of protocols – due to the flow in such cases is usually based not on the resource spent but on the psychological factor instead. The nodes owners do not benefit from making incorrect decisions, because it will lead to the system errors and will indirectly influence their status.
* Consensus protocol - the decisions are made in a coordinated manner by several nodes taking into consideration that some nodes might be malicious or work incorrectly.
* Consensus protocol + PoW/PoET or PoR/PoI mixture. There are the tasks (e.g. choosing a voting committee, setting an offer, dealing with the protocol’s stagnation, etc) that still need a leader.  However the decision is still made in a coordinated manner.

We believe **the consensus protocols or a mixture of the consensus protocols + PoR/PoI** suites us most. The consensus protocols are based on the state machine replication in the distributed environment (Leslie Lamport [12], [13] and Fred Schneider [14]). It is true that in order for two or more nodes to come to a common decision they should possess the same input date necessary for the decision making, to be in the same state, and have the same state changing rules. All the consensus protocol’s variations are aimed at ensuring the replication with the minimum necessary information transferred.

The Proof-of-Work and Proof-of-Elapsed Time type protocols put a great load on the clients’ nodes. This is highly undesirable in case of the low capacity equipment.
It is not advisable to use the leader-based protocol for the public ledger – it presupposes the individual trust to the leader. This is something to be avoided.

Here is a list of some consensus protocols [6]:
1. Paxos is a family of protocols for solving consensus in a network of untrusted nodes [15].  It is possible the data transfer between the nodes can be corrupted. The protocol includes the clients (those willing to get the consensus decision), voters, a proposer (more commonly known as a speaker) and a leader who is a proposer making when the consensus progress in a dispute.
2. Raft is a family of protocols designed as an alternative to Paxos [16]. Paxos implementation and verification are extremely difficult. Raft is easier to understand and implement. There are lots of implementations and visualisations with comprehensible specs in Go, C++, Java, and Scala.
4. PBFT – there are many PBFT modifications. They are:
   * classic PBFT [18]
   * Federated Byzantine Agreement (FBA) [17].
   * Zyzzyva [19]
   * Aardvark [20]
   * RBFT is an Aardvark enhancement on which Plenum is based (which is an upgraded RBFT and is used in Sovrin ledger and, thus, in Hyperledger as well [21])
   * Delegated Byzantine Fault Tolerance [22]
6. HoneyBadgerBFT (Hofburg Palace Vienna, Austria / October 24-28, 2016) - asynchronous BFT with the enhanced cryptography [6]
7. Hashgraph Virtual-Voting - asynchronous BFT, not PBFT [23]

The following table depicts the platforms’ description working on the above-mentioned consensus protocol families:

| Algorithm type           | Platform                               |
| ------------------------ | -------------------------------------- |
| Paxos                    | None                                   |
| RAFT                     | Kadena [https://github.com/kadena-io/] |
| PBFT                     | Hyperledger, Stellar, Ripple, Zilliqa  |
| RBFT (PBFT modification) | Sovrin, Hyperledger                    |
| dBFT (PBFT modification) | NEO                                    |

### 3.2. BFT/PBFT/ABFT/RBFT/DBFT algorithms comparison

In fact, there are only two comprehensive comparisons of the PBFT protocols [24] and [25]. It is a problem as there is no significantly important number of the objective reviews. The algorithms need to be studied more. The research [24] offers the review based on the experimental comparison of the various algorithms’ implementation using the BFT-Bench framework. Even though the experimental comparison does not provide fully reliable information (the implementation process might have allowed for some mistakes), we are utilizing it as it is one of few sources providing the comparative time estimates.

The picture displays the reviews/estimates for 4 nodes.

![Throughput single crash](./Consensus_Selection_02.jpg)

![Throughput delay](./Consensus_Selection_03.jpg)

RBFT and Aadvark are showing the best performance when there is a continuous message delay.

![Throughput flooding](./Consensus_Selection_04.jpg)

RBFT and Aadvark are still showing the best performance when flooding.

![Throughput complex scenario](./Consensus_Selection_05.jpg)

Aliph and PBFT are winners, however the performance when there are anomalies is better when compared to the normal conditions. This is weird.

The research [25] compares PBFT, Chain, RPFT, Aardvark, Aliph, Zyzzyva (only the fault-free version) having 4 nodes, 1 node can be faulty. The research shows the following results:

![Throughput single crash](./Consensus_Selection_01.jpg)

Zyzzyva obviously provided the best speed results (it makes sense as its algorithm is optimised for speed). Aardvark shows better results in terms of stability. Newer algorithms have not been compared.

The consolidated performance table for the different cases:

| Normal conditions  | Single replica crash           | Continuous delays and flood  | Delays and flood (5 clients)          | Overload (10 clients)       |
| ------------------ | ------------------------------ | ---------------------------- | ------------------------------------- | --------------------------- |
| Aliph              | PBFT, Aliph                    | RBFT                         | RBFT                                  | RBFT                        |
| Zyzzyva            | -                              | -                            | PBFT (unstable)                       | -                           |
| PBFT               | -                              | Aardvark                     | -                                     | Aardvark                    |
| RBFT               | -                              | -                            | -                                     | -                           |
| Aardvark           | Zyzzyva, RBFT, Aardvark, Chain | Zyzzyva, PBFT, Aliph, Chain  | Zyzzyva, Aliph, Chain                 | RBFT, Zyzzyva, Aliph, Chain |

The Honey Badger of BFT Protocols - is a new protocol that is not PBFT, but is pretty close to it. There is no objective review on it. The protocol spec states the following:
"Most fault tolerant protocols (including RAFT, PBFT, Zyzzyva, Q/U) don't guarantee good performance when there are Byzantine faults." Well, we have just seen it. The statement seems correct.
"Even the so-called "robust" BFT protocols (like UpRight, RBFT, Prime, Spinning, and Stellar) have various hard-coded timeout parameters, and can only guarantee performance when the network behaves approximately as expected - hence they are best suited to well-controlled settings like corporate data centers."

"HoneyBadger nodes can even stay hidden behind anonymizing relays like Tor, and the purely-asynchronous protocol will make progress at whatever rate the network supports."

The authors provide the following charts:

![Latency in log scale](./Consensus_Selection_06.jpg)

Latency vs throughput for experiments running HoneyBadgerBFT over Tor.

120 tx/s when working via Tor with the majority of the faulty nodes.

![Maximum throughput](./Consensus_Selection_07.jpg)

When there are more than 16 nodes, HoneyBadgerBFT provides the higher throughput than PBFT. When there are 64 nodes, the HoneyBadgerBFT's speed is 4-4.5 times better than PBFT's.

### 3.3. BFT drawbacks

1. Problem statement: the classic model works well in the small size groups, as it requires lots of messages to exchange.
   Solutions:
    * Sharding - the consensus group is divided into s smaller sub-groups. The diversity is still presents. There is plenty of the nodes to select from.
    * The selection criteria can be PoR-based for the consensus group. The consensus nodes are selected based on their reputation.

2. Problem statement: MACs (Method Authentication Codes) usage.
   Solutions:
    * Digital signatures, multisignatures or threshold signatures

3. Problem statement: the resilience to the Sybil attacks is unknown.
   Solutions:
    * Use PoR with the penalties (see below) for the consensus nodes.

4. Problem statement: DDoS.
   Solutions:
    * RBFT is immune to DDoS (see the charts above). HoneyBadgerBFT '''seems''' to be immune as well. Needs to be proved.

### 3.4. Conclusions

1. Purely speculative the Hashgraph Virtual-Voting should give the best speed. However it is '''patented''': "Because a distributed database system 100 is used, no leader is appointed among the compute devices … Specifically, none of the compute devices … are identified and/or selected as a leader to settle disputes between values stored in the distributed database instances … of the compute devices . Instead, using the event synchronization processes, the voting processes and/or methods described herein, the compute devices … can collectively converge on a value for a parameter." [https://www.swirlds.com/ip/]->[USPTO 9,529,923].
2. RBFT (or Plenum) may theoretically be used. Not the best peak performance, however looks solid. However the replica crashes are very often (up to 1/3 from the total quantity of the consensus nodes). PoR may actucally save us here. We will be disabling the crashing nodes. Plenum is implemented in Hyperleger Indy [https://github.com/hyperledger/indy-plenum/wiki].
3. HoneyBadgerBFT - some academia publications. The interesting idea and the fault tolerance are big pluses. The sources are available. No public reviews.

The ROMAD consensus is going to be based on BFT (more precisely dBFT). Some ideas from the HoneyBadgerBFT shall be taken as well.

# 4. Model
The following design principals will be used in ROMAD blockchain:
1. Directed Acyclic Graph. **Motivation:** the transactions can be done in parallel. It means better tx/s rate.
2. Sharding. **Motivation:** the sharding reduces the consensus time. Transactions finality becomes lower.
3. Voting is based on the Proof-of-Reputation + penalties. The voting PoR nodes have the certain hardware requirements (see below). **Motivation:** faulty or malicious nodes will get penalized quickly. The Sybil attack possibility is low.
4. Consensus type: dBFT.
4. Digital signatures. ECC-Shnorr (like in Zilliqa) or some ECDSA treshold option.

ROMAD will use Delegated Byzantine Fault Tolerant (dBFT) consensus model similar to NEO or Tendermint. This is a distributed system which is peer-to-peer based. All messages are broadcasted.

The ROMAD model has the following node types:
1. Ordinary Nodes - the ROMAD tokens owners. They can create the transactions, up or downvote for a specific delegate.
2. Delegates - they check each and every block. The reward is given for the block check. The requirements are:
 * the HDD space for the Ethereum blockchain (>324.15 GB) (more research on the Simple Payment Verification is needed - proof of inclusion, this will allow to reduce the HDD requirements) and ROMAD blockchain (>100 GB);
 * the high bandwidth Internet connection;
 * good CPU;
 * additional requirements may arise or the current requirements might even be decreased. This is not really important. What is important is that the delegate has no motivation to cheat. If they are unable to meet the requirements, they will never become the speaker and will never get the reward.

***There should not be many delegates***, otherwise the voting process will require the significant time to complete. Initially we plan to have ~100 delegates. This number may go up or down (e.g. NEO has 51 delegate).

dBFT assumes each block is verified by a number of delegates. When the block is being verified:
1. The speaker is selected from the delegates. This is the one that actually makes the calculations. The process is determined. Each delegate must become a speaker for the time slot given. The speaker's reputaton is taken into an account. The better the reputation, the more often this delegate becomes a speaker.
2. The other delegates are the verifiers. For the block to get verified, it needs to contain at least n-f signatures, where f = floor( (n-1)/3) ), where n is the number of the verifiers.

## 4.1. Delegates
To become a delegate one needs to register as a delegate. The registration is the special transaction. Remember the transactions are broadcasted, the other nodes shall get it fast enough.

## 4.2. Reputation
**The Nothing-at-Stake problem is the verifiers can do a mess if there is no penalty for their actions.** The mess example is, e.g. an invalid transaction confirmation. The common solution for the Nothing-at-Stake problem is the bond deposits [2]. This means the certain amount of the money is blocked on the verifier's account until it is proved the transaction does not create any conflicts to the blockchain.

ROMAD team is not currently considering the bond deposits to solve the Nothing-at-Stake problem. We use the reputation idea instead.

1. Initially each delegate gets a fixed reputation value ![Rho](./Rho.gif).

2. All reputation values are the same and do not depends on the number of tokens at delegate's stacke.

3. If the verifier gets dangerous for the network, its reputation automatically decrease using predifined rules (see. 4.3. Consesus).

## 4.3. Consensus
ROMAD consensus is round based. The round is given to the verifiers to process a single block. The block processing is atomic. When the round is over, the block is either verified or not. When it is verified, it is immediately available to the blockchain.

Consensus algorithm:
1. The nodes are creating the broadcasted transactions. Each transaction may have the optional digital signature.
2. The ordinary nodes do not save them.
3. The delegates are saving them.
4. Each delegate has a full copy of ROMAD blockchain and the certain data from Ethereum blockchain.
5. When there is a certain number of the unverified transactions, each delegate starts the algorithm for the next speaker selection.
6. The speaker generation algorithm state depends on the actual number of the transactions on the blockchain and the delegates' reputations.
7. The delegates are calculating the reputation independently. The upvotes, downvotes are taken from ROMAD blockchain and the tokens amounts are taken from Ethereum blockchain.
8. The delegate who has generated identifier becomes a speaker and forms up a "suggestion".
9. If for any reason the delegate does not form up a "suggestion", the unverified transations number grows up and the "suggestion" flag goes to the different delegate.
10. Remember, the delegates see the same blockchain state in more or less synchronized manner, so everyone knows whose turn is it now to form up a "suggestion".
11. The "suggestion" is signed with a secret key and is broadcasted.

![Proposal](./Eqn1.gif)

where:
  *	the speaker with the identifier p;
  *	has a reputation r;
  *	signed a block bi, view  = v;
  *	the block contains = block [the transactions hashes];
  *	![Block1](./Eqn3.gif) - the _block_ is signed with the secret key P_p of the user _p_.

12. Once the message from 11 is broadcasted, the delegates are verifying it with the certain rules (see below).
13. The delegates are sending the message:

![Response](./Eqn2.gif)

where:
  *	Response - Proposal Agree or Proposal Failed
  *	bi - block index
  *	view  = v;
  *	d - the delegate identifier
  *	r - the delegate reputation
  *	![Block2](./Eqn4.gif) - the _block_ is signed with the secret key P_d of the user _d_.
14. When there is a message Reposnse: Proposal Agree  from (𝑛 - 𝑓) delegates, every delegate understands there is a consensus and the full block is written on the blockchain.
15. The next round begins (goto 5) (yeah, goto haters!)

***If there are any violations on 12***, such as:
  * the data format for the transaction is invalid;
  * the delegate became a speaker out-of-order;
  * the transactions are already on the blockchain;
  * not all contract scripts transactions are completed;
  * double spent is detected;
  * the wrong link to the previous block (the fork attempt);
  * the speaker reputation is not corresponding to the one stated in the message;
  * the speaker reputation is 0.

***the block is considered invalid.*** The delegate that has detected this, does the following:
  * sends the Proposal Failed message.
  * does the ChangeView. It changes the View number so that the new speaker is selected.

When the other verifiers are getting the Proposal Failed message and there are at least (𝑛 - 𝑓) of them, they also send the ChangeView message.
  * Thus the malicious speaker is opted out for the different one;
  * the malicious speaker is penalized with decreasing β=1/e^((f-1)*2) , where f - is the fails number for the given speaker. A single fail, therefore, is of no importance, 2 fails decrease the reputation 8 times, 5 fails will completely nullify the reputation;
  * the Response messages are stored on the blockchain.

***If there is a timeout on 14*** and there are no (𝑛 - 𝑓) response messages
***then:***
  * Any delegate sends Response = ChangeView message
	* when the other delegates are getting the ChangeView message and there are at least (𝑛 − 𝑓) of them, they also send the ChangeView message.
	* The view_number is changes, the speaker is also changed.
	* No penalties for the speaker in this case.

The algorithm ensuers the immediate transactions availability when the consensus round is over.

# Questions & Answers
## 1. Is it possible that 2/3 of the delegates are malicious?

We plan to have ~100 delegates (like Tendermint). As the token owners shall vote independently, it means the malicious party has to have at least 66% of the ROMAD tokens to create its puppet delegates. This seems to be expensive.

## 2. Anything else?

The delegate must have the full ROMAD blockchain copy and at least a partial Ethereum blockchain copy. This is some form of the Proof-of-Space.

## 3. How do you plan to protect against Nothing-at-Stake?

The delegate's faulty or malicious behavior is stored on the blockchain. It is its reputation automatically decrease. Just 2 mistakes will lower the reputation 8 times. 3 mistakes will lower it 54 times. This way the penalized delegate will become a speaker less and less often.

# References
* [1] https://www.coinbureau.com/analysis/solving-blockchain-trilemma/
* [2] https://www.visa.com/blogarchives/us/2011/01/12/visa-transactions-hit-peak-on-dec-23/index.html
* [3] https://www.valuewalk.com/2018/01/transactions-speeds-cryptocurrencies-stack-visa-paypal/
* [4] https://thetangle.org/live
* [5] https://hackernoon.com/latency-and-finality-in-different-cryptocurrencies-a7182a06d07a
* [6] MILLER, A., XIA, Y., CROMAN, K., SHI, E., AND SONG, D. The honey badger of BFT protocols. Tech. rep., Cryptology ePrint Archive 2016/199, 2016.
* [7] https://blockchainhub.net/blockchains-and-distributed-ledger-technologies-in-general/
* [8] https://www.investopedia.com/news/public-private-permissioned-blockchains-compared/
* [9] https://blog.482.solutions/distributed-ledger-technology-and-its-types-ad76565ae76
* [10] https://medium.com/@BrettNoyes/public-permissioned-and-private-blockchains-3c32965e33c9
* [11] Yang W., Garg S., Raza A., Herbert D., Kang B. (2018) Blockchain: Trends and Future. In: Yoshida K., Lee M. (eds) Knowledge Management and Acquisition for Intelligent Systems. PKAW 2018. Lecture Notes in Computer Science, vol 11016. Springer, Cham
* [12] Pease, Marshall; Shostak, Robert; Lamport, Leslie (April 1980). "Reaching Agreement in the Presence of Faults". Journal of the Association for Computing Machinery. 27 (2). Retrieved 2007-02-02.
* [13] Lamport, Leslie (July 1978). "Time, Clocks and the Ordering of Events in a Distributed System". Communications of the ACM. 21 (7): 558–565. doi:10.1145/359545.359563. Retrieved 2007-02-02.
* [14] Schneider, Fred (1990). "Implementing Fault-Tolerant Services Using the State Machine Approach: A Tutorial" (PDF). ACM Computing Surveys. 22: 299. doi:10.1145/98163.98167.
* [15] Turner, Bryan (2007). "The Paxos Family of Consensus Protocols"
* [16] https://raft.github.io
* [17] https://towardsdatascience.com/federated-byzantine-agreement-24ec57bf36e0
* [18] M. Castro, B. Liskov, et al. Practical byzantine fault tolerance. In OSDI, volume 99, pages 173–186, 1999.
* [19] R. Kotla, L. Alvisi, M. Dahlin, A. Clement, and E. Wong. Zyzzyva: speculative byzantine fault tolerance. In ACM SIGOPS Operating Systems Review, volume 41, pages 45–58. ACM, 2007.
* [20] Allen Clement, Edmund Wong, Lorenzo Alvisi, Mike Dahlin, and Mirco Marchetti. 2009. Making Byzantine Fault Tolerant Systems Tolerate Byzantine Faults. In Proceedings of the 6th USENIX Symposium on Networked Systems Design and Implementation (NSDI’09). Boston, Massachusetts, 153–168.
* [21] https://www.evernym.com/wp-content/uploads/2017/07/The-Technical-Foundations-of-Sovrin.pdf],
* [22] https://steemit.com/neo/@basiccrypto/neo-s-consensus-protocol-how-delegated-byzantine-fault-tolerance-works
* [23] https://www.bitcoinforbeginners.io/cryptocurrency-reviews/hedera-hashgraph-review/
* [24] Divya GuptaLucas Perronne, Sara Bouchenak,  Part of the Lecture Notes in Computer Science book series (LNCS, volume 9687). https://link.springer.com/chapter/10.1007%2F978-3-319-39577-7_10
* [25] Alberto Montresor http://disi.unitn.it/~montreso/ds/handouts17/10-pbft.pdf
* [26] https://www.blockchain.com/btc/orphaned-blocks
* [27] https://www.cointelligence.com/content/tangle-dag-vs-blockchain/
* [28] https://steemit.com/cryptocurrency/@jimmco/byteball-vs-iota-battle-of-two-dag-cryptocurrencies
* [29] https://medium.com/@bitrewards/blockchain-scalability-the-issues-and-proposed-solutions-2ec2c7ac98f0
