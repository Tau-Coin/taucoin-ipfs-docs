# 选链机制

David adds: 
In the swarm, when peers send out (n+1)th blocks, the highest difficulty n-th block will win. However, when this blockchain's fork block happens prior to mutable range, it will send to human intervention for now. In the future, we will adopt voting system, which could be onchain id basis, pot power basis or pos power basis.

A normal node is the one continously online for more than 12 hours. Other than this, it is defined as new node.
正常节点的定义：任何节点只有连续在swarm中接受n+1区块12小时，才是正常节点。除此之外就定义为新节点。新节点的选链状态一直是在随机行走中的。 For new nodes, there is not mutable range, until it is online for 12 hours.

“最长链的切换“策略。在tau swarm peers中对n+1个区块进行表决时，接受节点A会收到许多P1，P2，P3...等的n+1区块的表决，这些区块的第n区块都有个积累难度值。含有第n区块的链的分叉点在12小时的mutable range内，难度值最大的第n区块b1将获得信任，含区块b1的区块链即便是一票也可以获得胜利，本质上是在swarm里面随机行走。但是区块链的位置分叉点落在mutable range 12小时切分点之前，b1将不会被切换，而转给人工处理。

## 节点pub的内容

理论上，节点之间只用pubsub两个topic就可以处理完成区块（包括链的投票、同步、新区块的同步）和交易（包括现存交易池的同步、新交易的同步）相关的信息交换。区块的topic是每个节点定时pub自己本地的一个root cid，该root cid连接到144个最新高度的区块，这里以mutable range=7为例说明；交易的topic也是每个节点定时pub自己本地的一个root cid，该root cid连接到一定数量的最高交易费的交易，比如前500笔交易费的交易，有了该root cid，其它节点可以按需下载一定数量的交易。由于所有之前获取过的内容，第二次获取会优先从本地拿，所以也不用担心重复带来的流量问题。

![blockrootcid]( https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/blockrootcid.jpg )  

![txrootcid]( https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/txrootcid.jpg )    

## 选链

### 同步阶段

一般新节点上线，会经过两个同步阶段：

投票同步阶段：这个阶段主要针对于新上线的节点或者之前同步过少部分区块的节点，本地区块高度距离社区最长链高度144个区块之外，由于需要同步mutable range之外的区块，因此需要慎重投票，在某个高度选出一个票数最多的区块，然后回溯同步。由于同步时间问题，回溯同步完成之后社区链可能又增长了不少，因此上述过程可能需要投票迭代多次，直至本地区块高度同步至所发现的社区最长链的144高度差以内，并进入下一阶段同步过程；

快速同步阶段：这个阶段主要针对本地区块高度低于发现的最长链高度144个区块之内的情况，这时需要同步的区块在mutable range之内，因此无需投票，可以快速同步。

### 实施步骤

![voting]( https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/voting.jpg )  

\1. 初始化，并进入节点发现阶段，发现到m个peer或者尝试足够的时间，结束节点发现阶段;

\2. 从m个peer中可以按照一定标准（比如历史行为信任度、peer id的矢量距离等条件），挑选出具有可信度和随机性的节点n个，以n=5为例;

\3. 收集这5个peer所pub的cid，并get解析出其对应的7个区块，出错或异常则转入1;

\4. 获取本地区块高度H1，从5个peer sub到的最大区块高度记为H2，比较H1和H2，如果H1>=H2，转到9；如果H2-H1>144，转到6;

\5. 挑选最长链最高高度H2对应的区块作为best block，转到8;

\6. 由height从高到低，选出得票数最多的区块的cid，比如图中k+1高度有5票最高票，获取其对应的区块作为best block，转到8;

\7. 分叉点超出mutable range异常，请求用户从社区获取当前最高的区块cid，获取对应的区块作为best block;

\8. 从best block回溯区块链并验证，出错或异常转到1，否则转到3;

\9. 开始挖矿。

#### 投票节点挑选方法

目前只有历史上有过挖矿记录的节点才可以投票；

以下方式暂不考虑：

基于Weak Subjectivity思想，挑选节点可以引入社区关系，同时与随机性相结合，挑选方法为：

1. 根据历史行为信用评分排名挑选，优先挑选信用评分高的，其中，白名单节点信用评分>0，陌生节点信用评分=0，黑名单节点信用评分<0；
2. 同信用评分情况下，比如都是陌生节点这种情况，根据与peer id的矢量距离远近挑选，要求矢量距离要满足一定的标准；
3. 同时，可以灵活根据评分等因素赋予每个节点的投票以相应的权重因子，信任节点与矢量距离近的节点也可以有一个固定的比例。
