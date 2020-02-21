# Block header中的地址

- Tau forger address
- Tau forger ipfs address
- Tau relay address

## Tau forger address

### 原因
兼容已上线的Taucoin地址系统

### 生成过程
Taucoin address的生成过程和Bitcoin一致：

1. Random(LuckyBoy) -> TAUPrivateKey-256 Bits, 32 Bytes
2. ECCSecpk256(TAUPrivateKey) -> TAUPublicKey-3 Types(04+ 64Bytes, 02||03+32Bytes)
3. Ripemd160(Sha256(TAUPublicKey)) -> TAUAddTemp-20 Bytes
4. DefinedNetworkNo-1 Byte+ TAUAddTemp -> DefinedAddTemp-21 Bytes
5. Sha256(Sha256(DefinedAddTemp)) -> HashDefinedAddTemp-32Bytes
6. Get 4 Bytes In HashDefinedAddTemp End-> HDAT[0], HDAT[1], HDAT[2], HDAT[3]
7. DefinedAddTemp+HDAT[3-0] -> FinalTAUAdd-25 Bytes
8. Base58encode(FinalTAUAdd) -> TaucoinFinalAddress-34 Bytes


## Tau forger ipfs address - 可选字段

### 原因
新节点连接的建立，需要Relay节点，但Relay节点不保证对端节点的连接过程，需要另一端的矿工节点IPFS ID，可以帮助建立新节点和已有矿工节点的连接。

Tau应用中存在多私钥应用的情况，用户会在同一台手机中使用多把Tau私钥挖矿，要求区块链数据的读写不存在因为Tau私钥不一致带来的障碍。Tau链的数据是建立在IPFS系统上的，也存在自有的地址系统，要求在单个手机上同时存在：1个IPFS地址系统，N个Tau地址系统。所以，Tau地址系统和IPFS ID系统不需要统一。

移动端的矿工节点往往存在NAT后，加入MultiAddress-IP，Port的意义并不大，保存其ID号即可。

### 生成过程
待补充.


## Tau relay address - 可选字段

### 原因
Tau应用是针对于移动网络而言的，移动客户端主要在NAT后，对于对称性NAT类型，建立两个节点的直接连接不可实现，需要借助于Relay来提供私密通道，达到'直连'的效果。

详细Relay内容可参见[Relay说明](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/p2p-network-relay-solution.md)

### 生成过程
待补充.


# Transaction中的地址

- Tau sender address
- Tau sender ipfs address
- Tau relay address

## Tau sender address

### 原因
兼容已上线的Taucoin地址系统

## Tau sender ipfs address - 可选字段

### 原因
加入IPFS Id相关，交易信息的查找，考虑借助于链上提供的信息。

## Tau relay address - 可选字段

### 原因
减少新Relay节点的加入Tau网络的成本，提高效率。
