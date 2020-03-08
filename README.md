# TAU - dForum on Mobile Cloud
Documentation on the Implementation of taucoin on ipfs

## Table of Contents

- [network structure](#network-structure)
- [data exchange](#data-exchange)
- [transactions pool](#transactions-pool)
- [voting](#voting)
- [nodes state](#nodes-state)


## NETWORK STRUCTURE

Making mobile phone behind firewall/nat to work as node requires relay nodes to connect mobile peers. The IFPS autorelay does not work as planned due to the spam on DHT. TAU used blockchain onchain info to provide higher quality relay information. When a TAU node getting online, it will read from local blockchain to connect to relays. 
After connecting to relay, the new node will build a swarm with peers that existing on chain either as miners or transaction senders. 
[Detail](doc/Tau-block-tx-structure-draft-cn.md)

## DATA EXCHANGE

TAU nodes exchange data through on one way: exchange (n+1) block hash among swarm peers. Each node will package own transaction along with valid txs from pool to build the n+1 block, then exchange with every peer's exchange requests. 
This is an innocation, will be more documented. 


## TRANSACTIONS POOL

Each node maintain own transaction pool. The pool information exchanging is through promoting n+1 block to other peers. The tx content is from locale tx pool. 

## VOTING

New node A will get many n+1 blocks from peers p1,p2,p3..pn. In A memory, for each n+1 block, A also maintains a future winninig block vectos will many n+1 blocks ranked by their expected time to win. When times comes, the top on the n+1 block vector will become new n-th block. When n-th block has two candidates with same difficulty level, choose high signature value option. This is to prevent two winning blocks with same difficulty level. 

## NODES STATE

When new nodes join the network, it has following stages:
1. Connect to relays - using software bootstrap and available blockchain content to connect relays. 
2. Build peer swarm - using software bootstrap and onchain info to connect peers.
3. Build wood wall - randomly asking for (n+1) blocks from on-chain peers from swarm, and verify blocks within the wall or the mutable range. When block comes in with higher difficulty, node will switch to higher difficulty chain.  
4. Build iron wall - online and verifying blocks from peers for 12 hours, wood wall becomes iron wall. When block comes in with higher difficulty, node will switch to higher difficulty chain only when it is within the mutable range. 
5. Transfer to human - in iron wall state, when higher difficulty chain forks happened out of the mutable range, it is considerred as an attack. so that require human envolve. 
All TAU nodes are working in stateless mode, the 12 hours in wood wall is the effort to sync with the blockchain and download state tree and necessary blocks. 
