# IPFS 文件操作相关
IPFS主要利用：`ipfs add `将数据(文件，文件夹)加入到IPFS网络文件系统中。
其他和文件操作相关的命令有：
- ipfs add, get, cat, ls, refs
- ipfs block, object, files, dag

在IPFS文件系统中，block是文件存储的最小单位，0.4.23版本中设计的是256KB，也就是说在IPFS中加入一个>256kb的文件，会被拆分为多个block文件，除非你使用`ipfs block put`来操作，每个拆分的block文件对应一个hash-cid，`cid`[说明](https://github.com/multiformats/cid)。在merkle dag的组织下形成一个最终的root hash。

## IPFS对文件大小的说明

在IPFS文件系统中加入文件过程中，并没有对文件大小有限制的选项，整个系统中除了本地IPFS系统配置的`StorageMax`选项外，并没有发现其他对文件系统有大小限制。`StorageMax`默认大小=10GB。

系统允许`add`一个大于`StorageMax`的文件，在文件`add`过程中，block对应的文件是实时拆分并进行`add`，IPFS目前的Merkle Dag文件组织形式不允许重复block的存在，只有当本地存储的block文件大于一定值时会启动`gc`操作，和`gc`操作的相关配置选项：`StorageGCWatermark`, `GCPeriod`。

## TAU 上链CID对应文件的限制

IPFS系统目前对加入网络中的文件大小，并无限制，这对于存储要求非常高，即使是网络式的分布式存储也是一个很大的问题，其中还需要考虑网络流量的成本和传输效率。

再者，最终进入系统中的文件操作都是按照hash-cid去进行的，从文件hash中无法获取文件点相关信息，尽管ipfs系统提供了`ipfs ls -v key`可以来获取文件hash下对应的block hash情况(Hash, Size, Name)，一般用户无法直接控制get的文件大小。

综合考虑，TAU应用对于信息交易中CID对应的文件大小进行限制，目前设定的大小为1MB，随着对移动端文字，图片，音频，视频的调研，这个限制值会做变动，会实时更新。