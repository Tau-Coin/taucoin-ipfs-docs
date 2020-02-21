## libp2p auto-relay 存在问题
 
#### libp2p auto-relay方案
    当libp2p打开auto-relay设置选项后，本地ipfs节点检测NAT状态，当检测到本地节点处于NAT，通过discover模块寻找中继节点。
    起初设置最大中继数目为3，当中继节点网络状态不好时，发现模块再次寻找中继节点，频繁调用发现模块引起DHT网络流量压力过大，
    请参考：https://github.com/libp2p/go-libp2p/issues/694。

#### libp2p foundation 优化方案
    不再通过DHT动态搜索中继节点，添加static relay nodes且设置连接的中继节点最大数目为1，以缓解DHT查询引起的网络压力。
    这种方案存在中心化问题，使static relay nodes网络压力过大。
    请参考：https://github.com/libp2p/go-libp2p/pull/705。


## TAU foundation relay solution

    1. foundation和社区提供可靠稳定的ipfs中继server，静态配置在软件中；ipfs节点启动后，会连接这些中继server。
    
    2. 在交易结构中提供可选中继地址字段；任何人都可创建ipfs中继服务器，把中继节点的multi-address填入到交易结构中，
    然后把该交易发布到tau forum blockchain网络中。相比与在区块中添加中继地址，通过交易发布中继服务器地址更便捷。
    矿工接收到包含中继地址的交易时，验证该中继multi-address数据格式正确。
    
    手机TAU节点接受到上链的包含中继地址的交易时，会把该中继地址记录在本地，从而引导手机ipfs节点连接到相应的ipfs中继服务器。