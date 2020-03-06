# 选链机制

## 选链

### 投票同步阶段

这个阶段主要针对于新上线的节点或者之前同步过少部分区块的节点，本地区块高度距离社区最长链高度144个区块之外，由于需要同步mutable range之外的区块，因此需要慎重投票，在某个高度选出一个票数最多的区块，然后回溯同步。由于同步时间问题，回溯同步完成之后社区链可能又增长了不少，因此上述过程可能需要投票迭代多次，直至本地区块高度同步至所发现的社区最长链的144高度差以内，并进入下一阶段同步过程；



### 实施步骤

#### 投票节点挑选原则

All swarm peers can vote. A new node will use XOR short distance to narrow the voting scope. The history miners or high POT power address can be given higher priority. To be explore...
