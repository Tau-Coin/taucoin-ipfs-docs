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


# MPT In TAU

MPT的操作类似于ETH, common.BytesToHash()计算key的hash方法修改为IPFS中cid hash方法, 并修改相关接口形参即可，数据库的读写更改为IPFS文件的读写。
