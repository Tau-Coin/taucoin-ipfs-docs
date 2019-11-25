# Block
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
 1   |version        | 1          |  "0x1"
 2   |option         | 1          |  "0x1"
 3   |chainid        | 32         |  "0x0"
 4   |blockno        | 8          |  "0x1"
 5   |basetarget     | 8          |  used for POT calculation
 6   |cumulativedifficulty    | 8       | fork judgement
 7   |generationsignature     | 32      | used for POT calculation
 8   |forger       | 20       | block forger address, for ipld index and show
 9   |timestamp    | 4        | unix timestamp
 10  |bootstrapid  | 32       | initial ipfs tau bootstrap nodes
 11  |previoushash | 32       |  necessary in blockchain
 12  |mmerkleroot  | 32       |  message transaction merkle root
 13  |cmerkleroot  | 32       |  coins transaction merkle root
 14  |signature    | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte
 15  |transactions | 32* 50   | ipfs cid format
 
 Total size: 1907 Bytes
 
# Transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
1   | version       | 1        |  "0x1"
2   | option        | 1        |  "0x1"
3   | chainid       | 32       |  "0x0"
4   | nounce        | 8        |  "0x1"
5   | timestamp     | 4        | tx timestamp
6   | sender        | 20       | for ipld index and show
7   | receiver      | 20       | tx receiver
8   | amount        | 5        | transfer amount
9   | fee           | 1        | transaction fee
10  | signature     | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte
  
Total size: 157 Bytes

# Modified Notes
## Block
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

```
独立出1-14的区块头信息部分，区块签名针对1-13部分；非矿工用户子只需要同步区块头信息+信息交易。
```
##### 问题讨论：
```
区块头中的交易部分是否需要和merkle root类似，独立为两部分(信息交易+转账交易)？
```

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

#### 多链应用下的交易类型，目前草稿：
### General info
 No              |  Key           | Size-Byte        |  Notes
 -------------------|----------------|------------------|----------------------
1   | version           | 1        | "0x1"
2   | option            | 1        | "0x1"
3   | chainid           | 32       | "0x0"
4   | nounce            | 8        | "0x1"
5   | timestamp         | 4        | tx timestamp
6   | sender            | 20       | for ipld index and show
7   | fee               | 1        | transaction fee
8   | signature         | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte

Size: 132 Bytes

### New chain transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
9   | name             | 20       | 论坛版块名字
10  | contact          | 32       | 版块管理员联系方式，暂定为telegram id
11  | title            | 144      | 论坛版块的标题，方便快捷浏览和内容定位
12  | description      | 32       | 论坛版块的描述细节，以IPFS Cid形式存在
  
Total size: 360 Bytes

### New name transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------------|----------------|------------------|----------------------
9   | contactname      | 32         | 论坛板块中可用于直接联系的方式，暂定为telegram id
10  | name             | 20         | 论坛板块中的昵称
11  | profile          | 32         | 论坛板块中的个人资料，以IPFS Cid形式存在；
  
Total size: 216 Bytes

### New message transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
9   | referid      | 32           | 被回复帖子的交易哈希
10  | title        | 144          | 回复贴的标题，方便快捷浏览和内容定位
11  | content      | 32           | 回复贴的具体内容，以IPFS Cid形式存在
  
Total size: 340 Bytes
