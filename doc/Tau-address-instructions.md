# Block header's addresses

- Tau forger address, the address for TAU address derived from private key
- Tau forger ipfs address, the address from IPFS initialization. In the mobile node, iForger is connected to tForger
- Tau relay multiaddress root, the set of relay that miner/forger observed while packing the n+1 block.

## Tau forger address

### concept
TAU original address compatibility and flexibity to use for different ipfs nodes.

### generation procedure
Taucoin address generation is similar to Bitcoin

1. Random(LuckyBoy) -> TAUPrivateKey-256 Bits, 32 Bytes
2. ECCSecpk256(TAUPrivateKey) -> TAUPublicKey-3 Types(04+ 64Bytes, 02||03+32Bytes)
3. Ripemd160(Sha256(TAUPublicKey)) -> TAUAddTemp-20 Bytes
4. DefinedNetworkNo-1 Byte+ TAUAddTemp -> DefinedAddTemp-21 Bytes
5. Sha256(Sha256(DefinedAddTemp)) -> HashDefinedAddTemp-32Bytes
6. Get 4 Bytes In HashDefinedAddTemp End-> HDAT[0], HDAT[1], HDAT[2], HDAT[3]
7. DefinedAddTemp+HDAT[3-0] -> FinalTAUAdd-25 Bytes
8. Base58encode(FinalTAUAdd) -> TaucoinFinalAddress-34 Bytes


## Tau forger ipfs address - optional field

### concept
新节点连接的建立，需要Relay节点，但Relay节点不保证对端节点的连接过程，需要另一端的矿工节点IPFS ID，可以帮助建立新节点和已有矿工节点的连接。
When new TAU nodes start, it will connect to relay. The relay needs a TAU has a ipfs node id to setup peer connection. 
Tau应用中存在多私钥应用的情况，用户会在同一台手机中使用多把Tau私钥挖矿，要求区块链数据的读写不存在因为Tau私钥不一致带来的障碍。Tau链的数据是建立在IPFS系统上的，也存在自有的地址系统，要求在单个手机上同时存在：1个IPFS地址系统，N个Tau地址系统。所以，Tau地址系统和IPFS ID系统不需要统一。
In TAU, one node can manager multiple private TAU key. So we need two address, tau id and ipfs id to make the communication and mining working. 

移动端的矿工节点往往存在NAT后，加入MultiAddress的意义并不大。 Since TAU nodes are mostly behind NAT/firewall, there is no meaning to store multiple address format for TAU node. 

### generation process
conform to default ipfs


## Tau relay multiaddress - optional

### concept
TAU nodes need relay on public internet to setup communication. TAU nodes are mostly mobile phones. They are behind firewall/nat and do not have a real IP address for public p2p communication. 

[Relay discussion](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/p2p-network-relay-solution.md)

### generation process
The miner will provide top 10 relay as default into the block header in IPFS cid format. 


# Transaction addresses

- Tau sender address
- Tau sender ipfs address
