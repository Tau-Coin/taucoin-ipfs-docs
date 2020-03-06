# TAU - dForum on Mobile Cloud
Documentation on the Implementation of taucoin on ipfs

## Table of Contents

- [network structure](#network-structure)
- [data exchange](#data-exchange)
- [transactions pool](#transactions-pool)
- [voting](#voting)
- [stateless](#stateless)


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

New node A will get many n+1 blocks from peers p1,p2,p3..pn. In A memory, for each n+1 block, it will have a list of n-th block, the believed right chain is based on the voting result of n-th voting from swarm peers. A also maintains a future winninig block vectos will many n+1 blocks ranked by their expected time to win. When times comes, the top on the n+1 block vector will become new n-th block for preparing local block. When n-th block has two candidates with same difficulty level, choose high signature value option. This idea is complex. Need more docs. 

## Stateless

All TAU nodes are working in stateless mode. Need more docs. 
