# Modified Notes

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

### core-vm-evm.go
depth
- 修改Call方法, depth相关逻辑都去除了, run调用去除, 去除run方法
- interpreter.go中去除Capturelog相关，这部分和logger相关，体现在Capture相关函数中,可以理解为捕获状态, gen_structlog.go
- internal tauapi,也有相关逻辑
- tau/Trace也有相关，实现了evm中的Capture接口；

1. 准备删除evm run方法
   run方法本身影响了evm.go自带方法，Call以及合约操作的几个函数

2. 准备去除evm.go中的Create和Create2方法；可以理解为创建合约的操作；之前删除Transactiondb中的调用，还有一部分逻辑存在于：:
	instructions.go -> 和jump_table.go 结合；
	instructions.go 定义了具体的execute方法；
	jump_table.go 定义的operation类有涉及execute方法
	runtime.go和cmd evm相结合
	delete create 方法

3. evm.go准备去除合约相关方法
- CallCode方法去除
	evm.go CallCode -> instructions.go opCallCode -> jump_table.go 

- DelegateCall方法去除
	evm.go CallCode -> instructions.go opCallCode -> jump_table.go  -> interpreter.go
    interpreter.go: NewEVMInterpreter 只留了一个JumpTable

- StaticCall方法去除，和DelegateCall一致

4. 删除EVM interpreters, interpreter成员

	前期已经将合约操作解决了，所以这里相当于直接删除即可

5. vmConfig

	evm.go中的逻辑，主要是NewEVM的初始化去除Config，涉及到
		/cmd/gtau/retesttau.go
	    /core/vm/instructions.go
		/tau/api_backend.go
		/tau/api_tracer.go
	还有一些test文件
	state_prefetch.go和blockchain.go的一些操作

### core-blockchain.go 
1. 去除vmConfig相关方法

	Processor中去除vmConfig
	ApplyTransaction中去除vmConfig

2. 去除NewBlockChain-> vmConfig传入

### core-vm-interpreter.go
只留了EVMInterpreter

### Contract.go 
已去除

### StateDB In IPFS
