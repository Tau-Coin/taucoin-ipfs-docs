# Block

 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
 1   |version        | 1          |  "0x1"
 2   |option         | 1          |  "0x1"
 3   |blockno         | 8          |  "0x1"
 4   |basetarget      | 8          |  used for POT calculation
 5   |cumulativedifficulty    | 8       | fork judgement
 6   |generationsignature     | 32      | used for POT calculation
 7   |forger       | 20       | for ipld index and show
 8   |timestamp    | 4        | unix timestamp
 9   |nodeid       | 32       | initial ipfs tau bootstrap nodes
 10  |previoushash | 32       |  necessary in blockchain
 11  |transactions | 32* 50   | ipfs cid format
 12   |signature    | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte
 
 Total size: 1813 Bytes
 ## Block结构待确定项：
 - 3-6-可简化代码逻辑以及应用复杂性；
 - 7-便于IPLD迅速索引得到出块者；
 - 9-和TAU的网络模型方案挂钩

# Transaction

 No              |  Key           | Size-Byte        |  Notes
 ----------------|----------------|------------------|----------------------
1   | version       | 1        |  "0x1"
2   | option        | 1        |  "0x1"
3   | nounce        | 8        |  "0x1"
4   | timestamp     | 4        | tx timestamp
5   | expiretime    | 4        | used for tx expiration timestamp
6   | sender        | 20       | for ipld index and show
7   | receiver      | 20       | tx receiver
8   | amount        | 5        | transfer amount
9   | fee           | 1        | transaction fee
10  | signature     | 65       | r: 32 bytes, s: 32 bytes, v: 1 byte
  
Total size: 130 Bytes

 ## Transaction结构待确定项：
 - 3-待确定项，解决交易的重放攻击；
 - 5-待确定项，解决交易的过期控制；
 - 6-待确定项，便于IPLD迅速索引得到发送者；

  
  
  
  
  
 


