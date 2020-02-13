# Modified Notes
## 20200213
### core-state_processor.go
ApplyTransaction
- 入口除了state_processor.go外，还有cmd/gtau/retesttau.go, miner/worker.go, core/chain_maker.go, 入口并不多
- tx.AsMessage, NewEVMContext, NewEVM, ApplyMessage后续更新
- 不区分IsByzantium
- delete receipt, 为了保持程序的正确性暂时保留NewReceipt部分

