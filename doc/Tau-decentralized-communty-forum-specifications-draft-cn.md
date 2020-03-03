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
	
	- 同步模式的设计和实现(Fast Start模式，Full Node模式)；
	- 社区最长链的识别和选择模块；
	- 区块,交易验证，回滚处理；
	- POT Forge模块；

    [区块大小，出块频率](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/pot.md)

## 4. APP UI层交互模块；

	4.1 APP UI端需求分析，设计和实现；

	4.2 交易的创建，数据存储和发送
	
	4.3 交互接口设计(交易，节点信息，节点状态)， 深入理解go-eth mobile部分，根据需求进行扩展；

	4.4 进程管理模块；

# 进展及计划

	- UI端基本完成了设计和实现，目前在做图片，音频和视频的切割和IPFS文件系统加载工作，这部分工作预计在20200306前完成，下一步做三种交易的创建，签名和发送工作；

	- 目前已构建了第一版的genesis block, 这部分构建还是基于eth leveldb来实现的，StateDB-MPT在IPFS文件系统中的存储还需要2-3天时间,在20200306前完成,完成StateDB-MPT后可以实现在IPFS文件系统中存储genesis block的账户状态；

	- Tau业务中和IPFS系统的pubsub功能已经封装好，网络部分等待联调；

	- gtau流程，需要完成config配置，service注册，tau node的启动，这部分是tau业务的基础架构，参考eth目前的实现，流程图和相关说明会在20200306前给出说明；

	- 交易池的管理，目前只保留了转账交易，单链中其他两种交易的输入和逻辑处理有待完善，具体方案会在20200306前出来；

	- blockchain业务，目前已完成的eth瘦身(evm去除了和vm相关，账户系统修改，POA共识),这部分业务实现较繁琐;
