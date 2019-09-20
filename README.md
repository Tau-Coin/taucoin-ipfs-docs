# taucoin-ipfs-docs
Documentation on the Implementation of taucoin on ipfs

## Table of Contents

- [network structure](#network-structure)
- [data exchange based on pubsub mechanism](#data-exchange-based-on-pubsub-mechanism)
- [block chain hash chain and hashc](#block-chain-hash-chain-and-hashc)
- [transactions transaction pool and hashl](#transactions-transaction-pool-and-hashl)
- [block synchronization flow chart](#block-synchronization-flow-chart)
- [sync mode in mobile node](#sync-mode)


## NETWORK STRUCTURE

![Imagetext](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/networkstructure.jpg)

## DATA EXCHANGE BASED ON PUBSUB MECHANISM

![pubsubmechanism](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/dataexchangebasedonpubsubmechanism.jpg)

## BLOCK CHAIN HASH CHAIN AND HASHC

![dst](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/blockchainhashchainandhashc.jpg)

## TRANSACTIONS TRANSACTION POOL AND HASHL

![Imagetext](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/transactiontransactionpoolandhashl.jpg)

## BLOCK SYNCHRONIZATION FLOW CHART

![sddt](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/blocksynchronizationflowchart.jpg)

## SYNC MODE
Anyone is able to run a taucoin node on their mobile. This means that you can participate in validating transactions and blocks on tauchain. To speed up user's mining process, fast mining mode is used. Also to prevent excessive dependence on TAU IPFS nodes and as an individual node in tauchain, entire blocks will be fetched from IPFS network. Later is called full node mode.

### Fast mining mode
In fast mining mode, lastest account state database-StateDBTAG is fetched from IPFS TAU nodes. In this process, the clients trust IPFS TAU nodes without any condition. When acount state database is done, clients can start mining after synchronizing several blocks.

### Full node mode
In full node mode, clients fetch each block from IPFS network, which made it possible to verify StateDBTAG. Also an obvious drawback is the low efficiency in synchronous process for each block's download and verification. We can validiate the StateDBTAG when block no reaches the height of marked StateDBTAG. 

Both fast mining mode and full node mode are concurrently existing when apk begins. Implementation will be improved later. 
