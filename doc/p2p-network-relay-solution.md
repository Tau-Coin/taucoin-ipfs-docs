## libp2p auto-relay 存在问题
 
#### libp2p auto-relay方案
    当libp2p打开auto-relay设置选项后，本地节点检测NAT状态，当检测到本地节点处于NAT后，通过discover模块寻找中继节点。起初设置最大中继数目为3，当中继节点网络状态不好时，发现模块再次寻找中继节点，频繁调用发现模块引起DHT网络流量压力过大，请参考：https://github.com/libp2p/go-libp2p/issues/694。

#### libp2p foundation 优化方案
    添加static relay nodes且设置连接的中继节点最大数目为1，以缓解DHT查询引起的网络压力。这种方案存在中心化问题，使static relay nodes网络压力过大。请参考：https://github.com/libp2p/go-libp2p/pull/705。


## TAU foundation relay solution

    ipfs手机节点接入到ipfs网络后，探测连接的对端节点是否为中继节点，把中继节点multiaddr上链。随着taucoin forum blockchain不断增长，会不断积累可靠的中继节点，从而引导手机ipfs节点连接这些可靠的中继节点。
    这种方法有两大优点：
    1. 与static relay方案相比，中继节点不再中心化；
    2. 通过forum blockchain的正反馈，更容易筛选出高可靠的中继节点。