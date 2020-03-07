# 开发代码的基础版本
```
go-ethereum-1.9.3
```

# 开发模块

## 1. 基础数据

### 区块

#### [区块结构](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-block-tx-structure-draft-cn.md)
	
#### [创世区块](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-genesis-block-cn.md)
	

### 交易

单链产品的交易类型：personal info tx, message tx, transfer tx

多链产品的交易类型：new chain tx, personal info tx, message tx, transfer tx

#### [交易结构](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-block-tx-structure-draft-cn.md)

#### [交易费分配](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-tx-fee-distribution-draft-cn.md)

	
## 2. 数据流(数据请求，获取，存储)

### 2.1 网络部分

第一阶段产品以独立IPFS进程形式存在，和链端以进程间通讯方式完成数据流；
	
	2.1 解决中继节点自动链接客户端，创建/p2p-circuit/链接问题，研究IPFS源码，查看p2p-circuit连接机制；

    2.2 深入理解go-ethereum p2p网络架构，采用ipfs pubsub实现p2p通信；

    2.3 go-ipfs RPC通信方案，完成RPC通信模块；

	2.4 区块数据的IPLD格式存储，解析，索引方案；

第二阶段产品以go-mobile为基础，开发go-ipfs-lib，以三方库的形式完成链端和IPFS端的交互；

### 2.2 StateMPT部分

[MPT In Tau](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-MPT-IPFS.md)

### 2.3 本地Block header的存储

目前Block header的设计满足: Block header chain <=> Block chain

## 3. Blockchain业务

### 3.1 交易池管理模块    


### 3.3 区块处理
	
- stateless framework
- 社区最长链的识别和选择模块；voting based right chain selection for n-th block.
- 区块,交易验证，回滚处理；on voting and MPT stateless mining, there is really no roll-back needed. It is just switch the stateroot.
- POT Forge模块；

[区块大小，出块频率](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/pot.md)

## 4. APP UI层交互模块；

	4.1 APP UI端需求分析，设计和实现；

	4.2 交易的创建，数据存储和发送
	
	4.3 交互接口设计(交易，节点信息，节点状态)， 深入理解go-eth mobile部分，根据需求进行扩展；

	4.4 进程管理模块；

# 进展及计划

```
1. 基于eth 框架(数据IO底层，共识不变)，实现交易池的交易收发(单链三种交易的统一)； 20200310
2. 基于eth框架(state存储于IPFS文件系统中, 共识不变)，实现交易收发；20200310
3. 基于eth框架(只保留blockchain数据库业务)，实现共识，区块，回滚机制的修改，成功出块； 20200321
    3.1 交易费的分配机制
    3.2 Mining, 共识机制, IPLD数据格式
    3.3 Blockchain业务
4. 基于IPFS框架，实现出块； //一个PC版的Forum 20200415
    4.1 网络部分的工作
		4.1.1 IPFS, lipp2p基础网络架构分析 //20200315
		4.1.2 Tau Node结合方案及代码实现   //20200331
		4.1.3 Tau Node协议实现 //20200415
    4.2 数据基于IPFS文件系统
		4.2.1 IPFS Core API for data filesystem  //20200331	
5. 集成Mobile环境，实现和UI端联调；//20200430

UI端的内容：

1. UI调整以及交易文件数据的处理 //20200310
2. 交易创建，发送的处理			//20200320
	2.1 传统Service实现UI端和链端的交易入池，新区块CID上报 
3. 区块链数据的加载				//20200415
	3.1 GoMobile实现UI端和IPFS交互 
```
