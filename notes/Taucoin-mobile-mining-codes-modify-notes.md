# Modified Notes
## 20200213
### core-state_processor.go

ApplyTransaction
- 入口除了state_processor.go外，还有cmd/gtau/retesttau.go, miner/worker.go, core/chain_maker.go, 入口并不多
- tx.AsMessage, NewEVMContext, NewEVM, ApplyMessage后续更新
- 不区分IsByzantium
- delete receipt, 为了保持程序的正确性暂时保留NewReceipt部分

### core-state_transaction.go

TransactionDb
- preCheck: buyGas -> buyFee
- preCheck: buyFee -> 改为交易费是否满足余额的检查，后续要修改Gas()方法
- 去除基础Gas计算, QuitIntrinsicGas
- 去除evm.Create操作,只有evm.Call操作，Call操作后续还要修改
- 去除退还交易费语句
