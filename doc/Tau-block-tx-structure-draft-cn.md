# Block Header - With IPFS DAG, block headers are all in a block. Transactions and state are all in the IPFS DAG. 
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
 1   |version        | 1          |  "0x1" as initial default, increase block through version 
 2   |option         | 1          |  "0x1" as initial default
 3   |chainid        | 32         |  "0x0" as TAU main chain or others for branch chain
 4   |blockno        | 8          |  "0x1" 
 5   |basetarget     | 8          |  for POT - Proof of Transaction calculation
 6   |cumulativedifficulty    | 8       | current consensus chain parameter
 7   |generationsignature     | 32      | for POT calculation, #7 x power x time
 8   |tforger      | 20       | block forger/miner address in TAU system, for IPLD index and display
 9*  |iforger      | 46       | block forger/miner address in IPFS system, for swarm peer connection
 10  |timestamp    | 4        | unix timestamp for winning the block package right
 11  |previoushash | 32       | link previou block
 12  |stateroot    | 32       | hash of state database MPT, merkle patrecia tree, in ipfs cid
 13  |txroot       | 32       | IPLD format to include transactions in ipfs cid
 14* | relaymaroot       | 72       | root of all ipfs relay multi-addressES observed, default is top 10
 15  |signature    | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte, when at #6 same difficulty, high signature number wins.

* 9,14 - optional

# Coin Wiring Transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
1   | version       | 1        |  "0x1" as default
2   | option        | 1        |  "0x1" as default
3   | chainid       | 32       |  "0x0" as TAU main chain or others for branch chain
4 *  | blockhash     | 32       |  "0x0" similar to EOS TAPOS, witness of the block at the mutable range point in a believed  chain
5   | nounce        | 8        |  "0x1" similar to ETH nounce to prevent replay transactions
6   | timestamp     | 4        | tx timestamp, tx expire in 12 hours
7   | tsender       | 20       | tx sender address in TAU system, for IPLD index and display
8 *  | isender       | 46       | tx sender address in IPFS system, for locating tx file in IPFS
9  | receiver      | 20       | tx receiver
10  | amount        | 5        | transfer amount
11  | fee           | 1        | transaction fee
15  | signature     | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte

4, 8 - optional; #12-#14 are message transactions fields.

# Personal info transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------------|----------------|------------------|----------------------
12  | contactname      | 32         | the defaul is your telegram id or other anything
13  | name             | 20         | nickname
14  | profile          | 32         | personal profile in ipfs cid format

# Message transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
12  | referid      | 32           | the past CID referred in new message
13  | intro        | 256          | the title/intro of the message
14  | content      | 33           | The content of the message in ipfs CID format

# Dev Discussion logs
### 20191118
- 考虑IPLD和区块数据信息统一以及效率问题，设计字段都有效保留；
- nodeid改名为bootstrapid
- 考虑多链结构下的区块信息流，加入chainid-genesis transaction hash字段以做区分；
---
### 20191125
- 考虑非矿工用户，只关心内容，进而减少其同步数据的流量；
- 根据上述需求，加入两个区分交易类型的merkle root：mmerkleroot, cmerkleroot;  
   - mmerkleroot: 信息交易(New chain transaction, persnonal info transaction, new message transaction)的MR
   - cmerkleroot: 代币转账交易的MR
---
### 20191126
- 区块中的交易merkle root，改为单层交易root, ![如图](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/txrootcid.jpg)
- Merkle root对于本应用而言，无优势，单层root结构可以无缝对接IPLD数据格式，效率也高；
---
### 20191204
- 考虑以太坊的设计，实现快速同步账户状态数据，增加stateroot字段；
```
stateroot是Merkle Patricia Trie的根哈希.
```
- MPT树的Value记录了账户状态(余额，Power-Nounce, 信息交易)，根据MPT来获取账户信息交易；
- 信息交易的获取根源从stateroot出发，因而将mtxroot和ctxroot合并为txroot；


##### 问题讨论：
```
Shared MPT的实现方案？
```

### 20200213
- 考虑IPFS效率问题，block header中加入MultiAddress信息：IP Info+ NodeId Info
- 交易中在以后版本中考虑中继节点信息

---
### 20200214
- 经过讨论，IPFS 可以利用dnt findpeer找到NodeId Info对应的IP Info, 只保留NodeId。

---
### 20200218
- 还原之前的讨论内容，block header中加入中继节点信息，该信息以MultiAddress形式加入。[详见说明](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/p2p-network-relay-solution.md)

---
### 20200221
- block header中加入矿工节点信息，该信息仅表现为IPFS cid形式。[详见说明](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-address-instructions.md)


## Transaction
### 20191118
- 考虑forum应用以及交易过期问题，去除expiretime字段；
- 考虑IPLD和区块数据信息统一以及效率问题，其他设计字段有效保留；
- 考虑多链结构下的交易信息流，加入chainid-genesis transaction hash字段以做区分；  
---
### 20191125
- New name transaction中加入profile字段，扩展为个人简介交易；
- new chain, message transaction中的abstract字段名修改为title；
- 信息类型由纯文字类型扩展到文字、图片、链接、视频四种类型，信息类型的由IPFS Cid区分；

##### 问题讨论：
```
IPFS Cid区分信息类型的方案：可以类似与CID version实现，message type+ cidv0
```
- message type:
  - 0 -> 纯文字
  - 1 -> 包含链接
  - 2 -> 包含图片
  - 3 -> 包含视频
  - ...
---
### 20200205
- new chain, message transaction中的title字段名修改为intro；
- intro由三部分組成：title+ 內容摘要(前144個bytes)+ IPFS Cid hash(图片hash, voice Hash等)
- IPFS Cid区分信息类型的方案暂定为：message type+ cidv0
- message type:
  - 0 -> 纯文字
  - 1 -> 包含链接
  - 2 -> 包含图片
  - 3 -> 包含视频
  - ...
---
### 20200214
- 交易中加入主链BlockHash, 以达到'TaPOS'。 [TaPOS说明](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/TaPoS.md)

---
### 20200218
- 上链的CID对应的文件大小，矿工要做一次验证以防攻击. [CID对应文件说明](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Taucoin-ipfs-cid-instructions-cn.md)

---
### 20200221
- 交易中加入可选字段：Sender节点信息，该信息仅表现为IPFS cid形式。
- 交易中加入可选字段：中继节点信息，该信息以MultiAddress的形式加入，为可选字段。
- 上述两个可选字段的加入，[详见说明](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-address-instructions.md)。

---

### 20200226
- 账户信息中只存在balance, nounce<=>power.
- 在现有的框架下，可以适应于多链系统，Shared MPT不再适用。
---

### 20200304
- 讨论的问题主要是解决IPFS中的Peer Routing问题
```
	1. 目前IPFS的Peer routing和Content routing是基于Kad DHT算法来实现的，在一定网络规模下效率有限

	2. 为了舍弃DHT查找矿工节点的问题，在区块链中尽可能记录已有矿工节点的信息(Peer Id+ Relay MultiAddress)

	3. 利用POT可预测出块的机制，矿工节点可以时刻进行区块的Forge, N+1区块的Forge是在其他节点请求下进行

	4. N+1区块保留Block中的Relay MultiAddress

	5. 对于打包出块的交易做如下变动:

		5.1 节点本身的交易，交易中需要添加到请求节点的Relay节点信息

		5.2 其他节点的交易，直接打包即可

		举例说明，A -> Rb <- B，B节点请求A节点N+1区块，A节点的本身交易需要添加Rb签名后打包
					其他节点通过节点B，已知Rb，进而可以Peer的链接；

				  A -> Rc <- C，C节点请求A节点N+1区块，A节点的本身交易需要添加Rc签名后打包
					其他节点通过节点C，已知Rc，进而可以Peer的链接；

	6. 增加Appendix Root Hash，其中放当前节点的其他交易信息，该Root Hash背后对应的是一个DAG结构的多笔交易

	7. N+1区块是实时请求，实时返回的结果，因为交易中添加Relay信息的不一致，导致N+1区块也是不同的结果
```

- 区块交易结构做如下变动，区块增加Appendix Root Hash，交易中再次确认Relay MutliAddress字段的重要性
---

### 20200305
- 讨论了节点连接和出块的具体实现，总结如下：
```
	1. 节点上线，利用Tau软件中配置的最初区块节点(交易信息)或者上次Mining退出时保留的最新区块，开启发现流程

		1.1 对于已在线过的节点，是否保留连接过的中继和Peers，或者优先连接

	2. 区块和交易信息中数据为上线节点提供了起始的Relay nodes和Tau链上的Peers；

	3. 连接和本节点网络通讯较好的几个中继节点后，借助Relay链路可以进一步连接Tau链上记录的Peer

		3.1 这部分的设计后期需要文档说明，Relay和Peer的挑选策略，进而组成一个高质量的Swarm网络

	4. Peer一旦建立连接，双方通过P2P数据协议获取特定节点的Block
		
		举例说明一些细节：
		
		新节点A请求B节点的下一个区块, 记做BBlock(n+ 1)
		
		4.1 BBlock(n+ 1)的区块时间是B节点下一个区块的预计出块时间

		4.2 BBlock(n+ 1)含有定制的B发出的交易，该交易中B节点需要填入和A节点连接的Relay节点MultiAddress

		B请求新节点A的下一个区块, 记做ABlock(n+ 1)，这种行为也是合法的，即便远落后于主链高度

	5. 通过Block(n+ 1)以及IPFS的Content Routing机制，可以获取每个Swarm peer的Block(n)

		5.1 对于Block(n+ 1)的处理，先拿Block(n+ 1)区块中记录的交易信息(打包的一级交易，Appendix root记录的交易),来丰富自己的交易池

		5.2 验证Block(n+ 1)合法性，不合法不放入出块时间列表

	6. 开始对Block(n)的投票流程，决定选择哪条链作为本节点的主链

	7. 对主链的确定实际更新了本地节点的Block(n),下次其他节点的请求可以回复调整Block(n+ 1)
		
		7.1 随着Block(n)的确定，以及Tau链中Stateless的设计，此时节点的Mining实际已经跟上了主链脚步

	8. 本节点同时维护一个出块时间列表，到达出块时间更新本地节点信息，本节点从N高度变更到N+1高度了，开始下一轮询问，转到Step 4
```

#### 多链应用下的交易类型，目前草稿：

### New chain transaction tbd
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
12  | name             | 20       | 论坛版块名字
13  | contact          | 32       | 版块管理员联系方式，暂定为telegram id
14  | intro            | 256      | 论坛版块的标题，内容摘要，抽象hash等
15  | description      | 33       | 论坛版块的描述细节，message type+ IPFS Cid形式存在
