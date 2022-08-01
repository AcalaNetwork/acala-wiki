# Composable DeFi Stack

### **Fully Composable DeFi Primitives in EVM and Runtime**

Smart Contract Dapps deployed in Acala EVM can directly use native and cross-chain assets such as DOT, ACA, aUSD, renBTC, etc. ERC-20 tokens deployed in EVM can also be made available at runtime level, to be listed in the DeX, or (by governance approval) to be used as fee tokens. For example, Ampleforth will deploy AMPL contracts on Acala EVM, which will be made available as a native token, so it can be used to pay transaction fees and listed directly on our DeX.

Smart Contract Dapps can directly use Acala DeFi primitives (DeX, stablecoin lending, and liquid staking), bridges, oracle infrastructure, native and cross-chain liquidity to compose various interesting DeFi applications, such as lending, special-purpose DeX, financial products based on staking and more.

The process is seamless for users and developers, but behind the scene native tokens and protocols (aka runtime modules) are made available in EVM in the form of precompiled contracts. A transaction initiated by a smart contract in EVM is translated into a Substrate transaction and signed by any Polkadot extensions using polkadot.js. The response will be processed by the SDK ([bodhi.js](https://github.com/AcalaNetwork/bodhi.js)) and converted into an Ethereum compatible format.

### Available Composable Contracts

These pre-compiled contracts are now made available to Acala EVM

* **Native and cross-chain tokens available in ERC20**: DOT, ACA, aUSD, XBTC, LDOT, RENBTC. Try it [here](../../../build/development-guide/smart-contracts/advanced/use-native-tokens.md).
* Oracle contract to get price feed: this exposes the [Open Oracle Gateway](https://wiki.acala.network/learn/basics/oracle) functionalities such as guaranteed Quality of Service. Try it [here](https://wiki.acala.network/build/development-guide/smart-contracts/advanced/use-oracle-feeds).
* On-chain auto-scheduler that enables use cases like subscriptions and recurring payments etc. Try it [here](../../../build/development-guide/smart-contracts/advanced/use-on-chain-scheduler/).
* Advanced contract deployment features like state renting to avoid scams and wastage of on-chain resources.
* More to come: we're gradually exposing more native functionalities to the Acala EVM, next to come is the DeFi primitives (DeX, stablecoin lending, and liquid staking).

Find out more on these contracts [here](https://github.com/AcalaNetwork/predeploy-contracts#predeployed-system-contract).
