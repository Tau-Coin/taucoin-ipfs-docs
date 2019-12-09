## 开发代码的基础版本
```
go-ethereum-1.9.3
```

## 开发模块
#### 1. 区块、交易相关
##### 区块

###### 1.1 [区块结构](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-Mobile-tx-block-structure-draft-cn.md)
	
###### 1.2 创世区块
	
###### 1.3 [区块大小，出块频率](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/pot.md)

##### 交易

单链产品的交易类型：personal info tx, message tx, transfer tx

多链产品的交易类型：new chain tx, personal info tx, message tx, transfer tx

###### 1.4 [交易结构](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-Mobile-tx-block-structure-draft-cn.md)
	
#### 2. 数据流(网络数据请求，获取，存储)

第一阶段产品以独立IPFS进程形式存在，和链端以进程间通讯方式完成数据流；
	
	2.1 解决中继节点自动链接客户端，创建/p2p-circuit/链接问题，研究IPFS源码，查看p2p-circuit连接机制；

    2.2 深入理解go-ethereum p2p网络架构，采用ipfs pubsub实现p2p通信；

    2.3 go-ipfs RPC通信方案，完成RPC通信模块；

	2.4 区块数据的IPLD格式存储，解析，索引方案；

	2.5 账户状态数据的存在形式，数据库选用；
	
第二阶段产品以go-mobile为基础，开发go-ipfs-lib，以三方库的形式完成链端和IPFS端的交互；

#### 3. POT共识模块
    
    3.1 深入理解go-eth共识实现架构，评估是否不改变架构实现POT;

#### 4. Forge模块；
	
	4.1 数据同步模块；
	
		- 社区最长链的识别和选择模块；
		
		- 多种同步模式的设计和实现(普通用户模式，矿工模式)；
		
		- 同步方案设计，考虑区块头，区块数据以及交易数据cid的同步效率；
	
	4.2 交易池管理模块；
	
	4.3 Forge管理模块；

#### 5. APP UI层交互模块；

	5.1 APP UI端需求设计和分析；
	
	5.2 交互接口设计(交易，节点信息，节点状态)， 深入理解go-eth mobile部分，根据需求进行扩展；

	5.3 进程管理模块；
	
#### 6. 节点管理模块

## 开发计划
正在以及近期需要解决的问题：
```
1.1-> 1.4
2.1
4.1
```