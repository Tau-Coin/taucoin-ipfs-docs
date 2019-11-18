# 选链机制

## 节点pub的内容

![blockroot]( https://github.com/Tau-Coin/taucoin-ipfs-docs/blob/master/imgfile/blockroot.jpg )  

每个节点pub的内容为mutable range=144个区块所对应的root cid，这里以mutable range=7为例说明。  

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