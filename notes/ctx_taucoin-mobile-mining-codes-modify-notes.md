- tx pool:移除以太坊特有的代码，如验证基础gas、gas相关的排序及限制、不同的升级阶段验证等；

默认配置改为开会讨论下来的配置

- blockchain:区块验证器去掉bloom相关的验证；
- 移除clique私有网络共识配置及挖矿逻辑
- 移除dao分叉代码
- 移除eip150、EWASM、Petersburg、Istanbul、Byzantium、Constantinople
- 移除checkpoint oracle和合约相关

