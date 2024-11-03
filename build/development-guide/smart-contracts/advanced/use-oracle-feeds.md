# Use Oracle Feeds

Acala's [Open Oracle Gateway](../../../../learn/acala-introduction/#open-oracle-gateway) allows multiple oracles to be deployed on the Acala network, leveraging Acala's DeFi optimized oracle infrastructure, and serving any DApps on Acala, Polkadot, Kusama and beyond. The Open Oracle Gateway ensures Quality of Service, that is oracle transactions are classified as system transactions, and are guaranteed to be included in a block.

This pre-compiled contract make querying various price feeds for various assets available inside Acala EVM. Currently the `getPrice` method returns the aggregated price for a given asset. More price feed options are being made available.

Contract source code here: [evm-examples/oracle](https://github.com/AcalaNetwork/evm-examples/tree/master/oracle).

> _**NOTE:**_ If you're running a local Acala node with EVM playground, you are like to have 0 prices for all of the tokens.

## Contract Address

Oracle contract address: `0x0000000000000000000000000000000000000801`

## Contract Methods

```javascript
// Get the price of the currency_id.
// Returns the (price, timestamp)
function getPrice(address token) public view returns (uint256, uint256);
```

## Try it on Playground

Navigate to the [EVM Playground](https://evm.acala.network/#/execute) - `Execute tab`.

Find the `Oracle Price Feed` contract and click the `Execute` Button.

![](<../../../../.gitbook/assets/Screen Shot 2021-02-03 at 12.55.05 PM.png>)

In the `token: address` field, enter the ERC20 contract address (in the example DOT address as `0x0000000000000000000000000000000000000802`) for the token of interest. Feeds are available for DOT, RenBTC and XBTC.

Find all available token contract addresses [here](https://wiki.acala.network/build/development-guide/smart-contracts/advanced/use-native-tokens).

![](<../../../../.gitbook/assets/Screen Shot 2021-02-03 at 12.27.16 PM.png>)

Click on the `Call` Button, `Call results` should show the price and timestamp.

![](<../../../../.gitbook/assets/Screen Shot 2021-02-03 at 12.27.29 PM.png>)
