# Composable DeFi Stack

### **Fully Composable DeFi Primitives in EVM and Runtime**

Smart Contract Dapps deployed in Acala EVM can directly use native and cross-chain assets such as DOT, ACA, aUSD, renBTC, etc. ERC-20 tokens deployed in EVM can also be made available at runtime level, to be listed in the DeX, or \(by governance approval\) to be used as fee tokens. For example, Ampleforth will deploy AMPL contracts on Acala EVM, which will be made available as a native token, so it can be used to pay transaction fees and listed directly on our DeX.

Smart Contract Dapps can soon directly use Acala DeFi primitives \(DeX, stablecoin lending, and liquid staking\), bridges, native and cross-chain liquidity to compose various interesting DeFi applications, such as lending, special-purpose DeX, financial products based on staking and more.

The process is seamless for users and developers, but behind the scene native tokens and runtime modules are made available in EVM in the form of precompiled contracts. A transaction initiated by a smart contract in EVM is translated into a Substrate transaction and signed by any Polkadot extensions using polkadot.js. The response will be processed by the SDK \([bodhi.js](https://github.com/AcalaNetwork/bodhi.js)\) and converted into an Ethereum compatible format.

