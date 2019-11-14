# IPLD 摘要
IPLD的官方说明有三个层次：
```
┌──────────────────────────────────────────────────┐
│                                                  │
│                   Schema Layer                   │
│ (advanced types for multi-block data structures) │
│                                                  │
├──────────────────────────────────────────────────┤
│                                                  │
│                 Data Model Layer                 │
│  (basic types for single-block data structures)  │
│                                                  │
├──────────────────────────────────────────────────┤
│                                                  │
│                   Block Layer                    │
│               (cid, data, codec)                 │
│                                                  │
└──────────────────────────────────────────────────┘
```
IPLD为区块链数据索引，引用和组织提供了有力的保障，目前已实现的BTC，ETH的IPLD数据格式，都是借助于定制版的Resolver进行的数据重载入IPFS系统，进而可以利用现有的IPFS工具进行相关的数据操作。

Taucoin想直接基于IPLD，直接将链上数据的存储于IPFS系统中，后续可以在终端、区块浏览器端直接进行数据的多级索引。要想达到此目的，需要熟悉IPLD的Block layer层内容。

## Block layer
**Documents:**

|     |      |
|-----|------|
| [Concept: Block](block-layer/block.md) | [block-layer/block.md](block-layer/block.md) |
| [Concept: Content Addressability](concepts/content-addressability.md) | [concepts/content-addressability.md](concepts/content-addressability.md) |
| [Concept: Multihash](block-layer/multihash.md) | [block-layer/multihash.md](block-layer/multihash.md) |
| [Concept: Serialization and Formats](block-layer/serialization-and-formats.md) | [block-layer/serialization-and-formats.md](block-layer/serialization-and-formats.md) |
| [Specification: CIDs](block-layer/CID.md) | [block-layer/CID.md](block-layer/CID.md) |
| [Specification: Dag-CBOR](block-layer/codecs/dag-cbor.md) | [block-layer/codecs/dag-cbor.md](block-layer/codecs/dag-cbor.md) |
| [Specification: DAG-JSON](block-layer/codecs/DAG-JSON.md) | [block-layer/codecs/DAG-JSON.md](block-layer/codecs/DAG-JSON.md) |
| [Specification: IPLD Selectors](selectors/selectors.md) | [selectors/selectors.md](selectors/selectors.md) |
| [Specification: Graphsync](block-layer/graphsync/graphsync.md) | [block-layer/graphsync/graphsync.md](block-layer/graphsync/graphsync.md) |

Block layer层中一些概念是比较好理解的，重点要理解PFS中的DAG结构，可以结合以下几个概念：
- Merkle-links
- Merkle-graph

### Merkle-links

Merkle-links是链接两个对象的方式。

link定义：
```
{ 
   "/" : "/ipfs/QmUmg7BZC1YP1ca66rRtWKxpXp77WgVHrnv263JtDuvs2k"
}
其中，"/"是key，"/ipfs/QmUmg7BZC1YP1ca66rRtWKxpXp77WgVHrnv263JtDuvs"是value；
```
两个对象简单例子：
```
{
  "foo": {
       "bar": "/ipfs/QmUmg7BZC1YP1ca66rRtWKxp77WgVHrnv263JtDuvs2k", // 不是一个链接
       "baz":
            {
               "/": "/ipfs/QmUmg7BZC11ca66rRtWKxpXp77WgVHrnv263JtDuvs2k"} // 是一个链接
            }
}
```
其中，link包含在对象baz中，而baz本身是foo的一个对象成员，进而是实现多级的索引。
### Merkle-graph
Merkle-links产生了对象间的互相引用，当这种引用关系变得复杂时，进而会产生graph的结果，这个graph也可以理解为tree。
### Merkle-DAG
Merkle-DAG保证了引用间的有向性，但是不会产生环，进而可以理解为Merkle-graph的升级版。
### 其他工具-IPLD Serialization and Formats
Merkle-DAG的世界需要解决表达问题，这种表达包括两部分：
- 计算机世界的存储，网络传输，对应了编码以及序列化方法。
- 现实世界的用户展示，对应了以何种数据格式展现给用户。 
这两种表达展现方式往往是绑定在一起的。

### 表现形式-Merkle-paths
Merkle-paths可以实现对象内遍历，也可以实现跨对象遍历。
Merkle-DAG一旦产生，那么其中文件路径的表达是需要解决的问题，Merkle-paths便孕育出身，它是一种unix风格的路径表达方式：
```
/ipfs/QmNu9vByGwjdnvRuyqTMi35FQvznEQ6qNLVnBFNxvJA2ip/a/b/c/d
```
# IPLD TAU
[交易和区块结构的详细信息](https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/doc/Tau-Mobile-tx-block-structure-draft-cn.md)
## Method
参考 [ipfs dag put相关操作](https://docs.ipfs.io/reference/api/cli/#ipfs-dag-put)
## Usage and Examples
```
ipfs dag get blockid
```
you will get:
```
{
   "version": 0x1,         
   "option": 0x1,           
   "blockno": 0x1,          
   "basetarget": 0x1,        
   "cumulativedifficulty": 0x1,   
   "generationsignature": 0x1,  
   "forger":  "0xe0e6c781b8cba08bc8407eac0101b668d1fa6f49"
   "timestamp": 1573451947,             
   "nodeid": "0x111b6113c9d3c319716da3cf057da24acf0fef897f4103d8a362d7bda42217db"   
   "previoushash": "0x123c6abcc9d3c319716da3cf057da24acf0fef897f4103d8a362d7bd274b7423b"
   "transactions": [txhash1, txhash2…txhash50]
   "signature": {
                    "signaturer":  "0x981b6223c9d3c319716da3cf057da84acf0fef897f4003d8a362d7bda42247db"
                    "signatures":  "0x66be134c4bc432125209b5056ef274b7423bcac7cc398cf60b83aaff7b95469f"
                    "signaturev":  "0x0"
                }
}
```

```
ipfs dag get blockid/forger
```
you will get `0xe0e6c781b8cba08bc8407eac0101b668d1fa6f49`.

```
ipfs dag get blockid/transactions
```
you will get `[txhash1, txhash2…txhash50]`.

```
ipfs dag get blockid/transactions/0 | ipfs dag get blockid/transactions/txhash1 | ipfs dag get txhash1
```
you will get:
```
{
   "version": 0x1,     
   "option": 0x1,      
   "nounce": 0x1,      
   "timestamp": 1573451947,  
   "expireTime": 1573461811,  
   "sender": "0xe0e6c781b8cba08bc8407eac0101b668d1fa6f49"  
   "receiver": "0xe0e6c781b8cba08bc8407eac0101b668d1fa6f49"  
   "amount": 10000,   
   "fee": 100,    
   "signature": {
                    "signaturer":  "0x981b6223c9d3c319716da3cf057da84acf0fef897f4003d8a362d7bda42247db"
                    "signatures":  "0x66be134c4bc432125209b5056ef274b7423bcac7cc398cf60b83aaff7b95469f"
                    "signaturev":  "0x26"
                 }
}
```
