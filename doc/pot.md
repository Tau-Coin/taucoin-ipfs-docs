### 12 hours as mutable range, transaction expiry time, voting, TAPOS

TAU 设计最大程度避免使用常数。任何一个常数的出现，也将获得最大程度的使用。12小时是个关键常数用于如下几个部分：mutable range的切分点，交易过期时间，TAPOS的区块索引。


In the swarm, when peers send out (n+1)th blocks, the highest difficulty n-th block will win. However, when this blockchain's fork block happens prior to mutable range, it will send to human intervention for now. In the future, we will adopt voting system, which could be onchain id basis, pot power basis or pos power basis. 

A normal node is the one continously online for more than 12 hours. Other than this, it is defined as new node.  
正常节点的定义：任何节点只有连续在swarm中接受n+1区块12小时，才是正常节点。除此之外就定义为新节点。新节点的选链状态一直是在随机行走中的。
For new nodes, there is not mutable range, until it is online for 12 hours. 

“最长链的切换“策略。在tau swarm peers中对n+1个区块进行表决时，接受节点A会收到许多P1，P2，P3...等的n+1区块的表决，这些区块的第n区块都有个积累难度值。含有第n区块的链的分叉点在12小时的mutable range内，难度值最大的第n区块b1将获得信任，含区块b1的区块链即便是一票也可以获得胜利，本质上是在swarm里面随机行走比较难度。但是区块链的位置分叉点落在mutable range 12小时切分点之前，b1将不会被切换，而转给人工处理。


### 关于出块时间与出块频率

以太坊关于出块时间有下面一段话：

​	12 second block time: 12 seconds was chosen as a time that is as fast as possible, but is at the same time substantially longer than network latency. A 2013 paper by Decker and Wattenhofer in Zurich measures Bitcoin network latency, and determines that 12.6 seconds is the time it takes for a new block to propagate to 95% of nodes; however, the paper also points out that the bulk of the propagation time is proportional to block size, and thus in a faster currency we can expect the propagation time to be drastically reduced. The constant portion of the propagation interval is about 2 seconds; however, for safety we assume that blocks take 12 seconds to propagate in our analysis.

里面提到的比特币出块时间的研究论文参考：https://tik-old.ee.ethz.ch/file//49318d3f56c1d525aabf7fda78b23fc0/P2P2013_041.pdf

因此，根据上述研究可知，在p2p环境中，区块的传播时间和区块大小有关。而在交易大小接近的情况下，区块的传播时间是和交易笔数是正比的，因此出块时间与出块频率的制定实际上是个tps问题。

移动节点制约tps的因素：1、移动网络，移动网络环境要考虑到比非移动网络环境差；2、中继连接，手机节点在ipfs环境中，大多会通过中继的方式连接，这种中继可认为是双倍的链路；3、tau的区块为ipld去中心化结构，ipfs中区块的传播不像传统的p2p作为整体传播，区块里交易是需要分多次get；4、历史区块同步，考虑到手机处理区块的速度比较慢，交易的验证慢，因此如果tps太高的话，一个新节点要历史区块，会有很大的工作量。

根据目前以太坊区块平均交易数为140左右，平均出块时间为15s左右，tps可以认为是9。那么移动节点的tps定为1的话，应该是比较安全的。同时考虑到区块头等载荷，高频低交易量比低频高交易量的交易负载比要低，也即是同样是1tps，5txn/5s会比20txn/20s交易负载比低，因此我们可以选择10txn/10s，也即是10s的出块时间，每个区块10笔交易。

### 出块参数更改

原先的出块时间为5min，也即是300s，相关调整参数为AVERTIME = 300，MINRATIO = 265，MAXRATIO = 335。

出块时间10s的话，相关调整参数可更改为AVERTIME = 10，MINRATIO = 9，MAXRATIO = 11。

### 修改日志

| 时间     | TPS  | CAP  | AVERTIME | MINRATIO | MAXRATIO | REASON |
| -------- | ---- | ---- | -------- | -------- | -------- | ------ |
| 20190601 | 1/6  | 50   | 300      | 265      | 335      | *      |
| 20191128 | 1    | 10   | 10       | 9        | 11       | *      |
| 20191202 | 1/6  | 5    | 30       | 27       | 33       | 1      |

1. 考虑到区块头的大小，交易量不能太低，否则区块运载比太低，同时保持1/6的tps。
