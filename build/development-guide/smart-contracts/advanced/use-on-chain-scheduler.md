# Use On-chain Scheduler

### Contract Address

ScheduleCall contract address: `0x0000000000000000000000000000000000000808`

### Contract Methods

```javascript
// Schedule call the contract.
// Returns the task_address(block_number, index).
function scheduleCall(address contract_address, uint256 value, uint256 gas_limit, uint256 storage_limit, uint256 min_delay, bytes memory input_data) public returns (uint256, uint256);
```

### Example \(TODO\)

