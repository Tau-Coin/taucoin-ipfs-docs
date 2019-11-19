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
 12  |transactions | 32* 50   | ipfs cid format
 13  |signature    | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte
 
 Total size: 1843 Bytes
 
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

## Transaction
### 20191118
- 考虑forum应用以及交易过期问题，去除expiretime字段；
- 考虑IPLD和区块数据信息统一以及效率问题，其他设计字段有效保留；
- 考虑多链结构下的交易信息流，加入chainid-genesis transaction hash字段以做区分；  
- 多链应用下，需要设计的交易类型，目前草稿：

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
11  | abstract         | 144      | 论坛版块的信息摘要，方便快捷浏览和内容定位
12  | description      | 32       | 论坛版块的描述细节，以IPFS Cid形式存在
  
Total size: 360 Bytes

### New name transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------------|----------------|------------------|----------------------
9   | contactname      | 32         | 论坛板块中可用于直接联系的方式，暂定为telegram id
10  | name             | 20         | 论坛板块中的昵称
  
Total size: 184 Bytes

### New message transaction
 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
9   | referid      | 32           | 被回复帖子的交易哈希
10  | abstract     | 144          | 回复贴信息的前144个字节，方便快捷浏览和内容定位
11  | content      | 32           | 回复贴的具体内容，以IPFS Cid形式存在
  
Total size: 340 Bytes
  
  
  
  
 


