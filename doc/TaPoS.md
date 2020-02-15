关于在交易中增加ref-block-hash字段，也即是在交易中增加主链区块的hash的机制，可称为TaPoS(Transaction as Proof of Stake)，NXT/BitShare/EOS已有类似的应用，[参考](https://talk.peercoin.net/t/can-tapos-recent-block-hashes-in-transactions-be-added-to-peercoin-to-improve-fork-resolution/8089)。具体说明如下：

1. ref-block-hash是可选字段，可为空或者引用的主链上的区块hash，考虑到taucoin的mutable range机制，可默认head block - mutable range点的block hash，又考虑到具体的实现效率，可考虑限制只能引用最远范围为2 * mutable range的block hash；
2.  在交易中增加引用主链区块哈希，理论上像一种投票，使用户的交易与链绑定，客观上可以起到设别并维护社区主链的效果；
3. 这种方式还可以防止分叉区块链上出现大量交易记录，使得系统能感知到用户是否在分叉出来的区块链上，随着时间的推移，所有用户最终直接确认区块链，这使得难以伪造假冒链，因为假冒链将无法从合法链路大量迁移交易，也容易被识别出来；