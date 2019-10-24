# IPLD
先看一下官方说明下的三层结构：
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
其中一些概念是比较好理解的，要理解IPFS中的DAG的结构需要结合以下介个概念：
- Merkle-links
- Merkle-graph

### Merkle-links
Merkle-link是链接两个对象的方式。

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
### IPLD Serialization and Formats
Merkle-DAG的世界需要解决表达问题，这种表达包括两部分：
- 计算机世界的存储，网络传输，对应了编码以及序列化方法。
- 现实世界的用户展示，对应了以何种数据格式展现给用户。 
这两种表达展现方式往往是绑定在一起的。

### Merkle-paths
Merkle-paths可以实现对象内遍历，也可以实现跨对象遍历。
Merkle-DAG一旦产生，那么其中文件路径的表达是需要解决的问题，Merkle-paths便孕育出身，它是一种unix风格的路径表达方式：
```
/ipfs/QmNu9vByGwjdnvRuyqTMi35FQvznEQ6qNLVnBFNxvJA2ip/a/b/c/d

```