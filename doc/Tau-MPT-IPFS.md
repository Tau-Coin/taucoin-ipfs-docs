# MPT In ETH

## Trie
1. Trie.New，先初始化Trie.db-Trie对应的database，并且从数据库中加载第一个rootnode

2. trie中的node代表了MPT的一个节点： 
	- 空节点  （hashNode） -> 类型断言强转用处
	- 叶子节点（valueNode）-> 目前只保留了Balance和Nonce<=>Power
	- 分支节点（fullNode ）
	- 扩展节点（shortNode）

3. Trie中的Get, TryGet, tryGet, Update, TryUpdate, insert, Delete, TryDelete, delete函数都是针对于MPT的原生操作；
		
4. resolve, resolveHash函数: 从数据库中根据key hash获取一个node
```
	func (t *Trie) resolveHash(n hashNode, prefix []byte) (node, error) {
		hash := common.BytesToHash(n)
		if node := t.db.node(hash); node != nil {  //t.db.node(hash) -> 一个核心点
			return node, nil 
		}   
		return nil, &MissingNodeError{NodeHash: hash, Path: prefix}
	}
```
 t.db.node(hash) 从数据库中获取相应的node，这个数据库包括缓存以及硬盘上的，后续的MPT in IPFS是需要修改的；
 
5. Hash, Commit, hashRoot获取root hash: 获取root hash的过程其实是一个MPT node -> 数据库存储的一个过程，也需要确认修改；

## SecureTrie

secureTrie是在Trie的基础上包了一层key，可以通过地址直接进行操作, 并且增加了一层key的缓存;


1. GetKey -> 可以通过key直接获取node信息，这个key可以理解为数据库中的key
``` 
	func (t *SecureTrie) GetKey(shaKey []byte) []byte {
   		if key, ok := t.getSecKeyCache()[string(shaKey)]; ok {
       		return key 
   		}   
   		key, _ := t.trie.db.preimage(common.BytesToHash(shaKey))
   	 	return key 
	}
```
2. db.preimage可以理解为一层缓存，在secureTrie中Commit函数中调用了db.insertPreimage，实际是写缓存一个操作。


# TAU Data

MPT的操作类似于ETH, common.BytesToHash()计算key的hash方法修改为IPFS中cid hash方法, 并修改相关接口形参即可，数据库的读写更改为IPFS文件的读写。

## State MPT获取过程

	1. 入口为Voting后最长链区块的Root Hash，该Hash对应了MPT的第一个根节点内容
	2. 通过第一个根节点内容，以MPT搜索方式可以获取某个账户的状态（余额+Power），在搜索过程中，本地IPFS会缓存MPT路径上的文件内容，Tau不做特殊处理，这些文件的管理交给IPFS本身
	3. 当获取到多个账户状态时，本地内存中的MPT也会有一定的膨胀，MPT节点会有相应的增多
	4. 当本地修改了某个账户信息时，会以MPT搜索方式进行MPT节点内容的修改，之后需要将这些修改的节点Add到IPFS文件系统中，以供其他节点使用
	5. 退出Tau时，本地内存中的MPT不会进行本地化存储，再次进入Tau时，根据上述过程继续获取账户的最新状态

## DB In Tau

	1. Tau区块链业务只保留当前最高区块Hash信息, 其他链端信息都存储于IPFS文件系统中
