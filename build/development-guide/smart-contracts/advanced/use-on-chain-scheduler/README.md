# Use On-chain Scheduler

You can use the on-chain scheduler contract to schedule a recurring call to execute a given smart contract. Read more on use cases for the on-chain auto-scheduler [here](https://wiki.acala.network/learn/basics/acala-evm/acala-evm-composable-defi-stack/on-chain-scheduler).

Contract source code [here](https://wiki.acala.network/learn/basics/acala-evm/acala-evm-composable-defi-stack/on-chain-scheduler).

Note: the `min_delay` is minimum number of blocks after current blocks before the scheduled call will be called. i.e. Pass 0 means it will be scheduled to be called on next block. Calling `scheduleCall` with `min_delay = 5` on block 10 will scheduled the call on block `10 + 1 + 5 = 16`. If the block are full or there are too many other scheduled tasks, the scheduled call could be deferred to later blocks until there is enough remaining spaces in the block.

## Contract Address

ScheduleCall contract address: `0x0000000000000000000000000000000000000808`

## Contract Methods

```javascript
// Schedule call the contract.
// Returns the task_address(block_number, index).
function scheduleCall(
  address contract_address, // The contract address to be called in future.
  uint256 value, // How much native token to send alone with the call.
  uint256 gas_limit, // The gas limit for the call. Corresponding fee will be reserved upfront and refunded after call.
  uint256 storage_limit, // The storage limit for the call. Corresponding fee will be reserved upfront and refunded after call.
  uint256 min_delay, // Minimum number of blocks before the scheduled call will be called.
  bytes memory input_data // The input data to the call.
)
public
returns (uint256, uint256); // Returns a task id that can be used to cancel or reschedule call.
```

## Example

[Here's](https://github.com/AcalaNetwork/evm-examples/tree/master/scheduler) a recurring payment contract example using the scheduler.

## Tutorial

Follow this tutorial to write and deploy a basic automatic subscription contract on Acala EVM.

