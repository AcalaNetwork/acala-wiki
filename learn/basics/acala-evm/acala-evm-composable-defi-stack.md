# Acala EVM - Composable DeFi Stack

## Acala EVM - Code Name: Project Bodhi

Acala and all Substrate-based chains are fundamentally different from Ethereum. If we are trying to emulate an Ethereum node, we will suffer from the worst of both worlds. It will be a step backward for us to inherit all the restrictions from a legacy blockchain platform.

We see EVM as one part of the Acala/Substrate/Polkadot, but EVM in itself does not provide a categorically different experience. Acala EVM will try to achieve these design goals

1. Enable users to have a complete full-stack Acala \(and Substrate\) experience seamlessly with a single wallet.
2. Enable protocol composability for EVM and runtime
3. Enable developers to develop and deploy DApps on Acala with great tooling support.

The Acala EVM delivers the following benefits from the best of both Ethereum and Substrate platforms:

### **1. Fully Composable DeFi Primitives in EVM and Runtime**

Smart Contract Dapps deployed in Acala EVM can directly use native and cross-chain assets such as DOT, ACA, aUSD, renBTC, etc. ERC-20 tokens deployed in EVM can also be made available at runtime level, to be listed in the DeX, or \(by governance approval\) to be used as fee tokens. For example, Ampleforth will deploy AMPL contracts on Acala EVM, which will be made available as a native token, so it can be used to pay transaction fees and listed directly on our DeX.

Smart Contract Dapps can soon directly use Acala DeFi primitives \(DeX, stablecoin lending, and liquid staking\), bridges, native and cross-chain liquidity to compose various interesting DeFi applications, such as lending, special-purpose DeX, financial products based on staking and more.

The process is seamless for users and developers, but behind the scene native tokens and runtime modules are made available in EVM in the form of precompiled contracts. A transaction initiated by a smart contract in EVM is translated into a Substrate transaction and signed by any Polkadot extensions using polkadot.js. The response will be processed by the SDK \([bodhi.js](https://github.com/AcalaNetwork/bodhi.js)\) and converted into an Ethereum compatible format.

### **2. Flexi Transaction Fees**

On Acala, fees can be paid in any accepted tokens e.g. ACA, aUSD, DOT, BTC etc. More tokens can be supported as native fee tokens with governance approval.

Behind the scene, Acala DeX is being used as a unified liquidity pool for settling the fees into the network token, but the experience is completely transparent to users and developers.

### **3. Single Wallet, Single Account Experience**

Users can use **one extension/wallet**, and **a single Substrate account** to interact with the Substrate runtime, contracts in EVM, and wasm contracts or a hybrid of these. If a user wants to use a particular Ethereum address, then simply link it with his/her Substrate address \(basically proving the user owns both addresses\), thereafter the user can just use the Substrate account to sign any Ehtereum transactions seamlessly.

This allows users to use all functionalities within Acala and cross-chain capabilities without managing multiple accounts or wallets.

### **4. Keep Nodes Lightweight while Query Capability Intact**

We retain the standard Substrate node which is lightweight and easily maintainable. For querying transactions and event logs, we offer an indexer node that is open-sourced and anyone can run a copy of it like a full-node.

For convenience, we’d offer one docker image to run both nodes, but it is important that there’s a choice for people who want to run one or another for their purposes.

### **5. Upgradeable Smart Contracts.**

In Acala EVM, developers no longer need to write complex migration contracts to fix bugs or make improvements to existing applications. The contract maintainer can simply send a transaction with the new contract byte-code to seamlessly upgrade the contracts, without the need to `migrate` users nor liquidity.

There’s also a two-staged deployment process to reduce the risk of directly testing on mainnet with the public.

* **Deployed Private Contract**: Once a contract is deployed, it is by default private to the public and visible only to the `Contract Maintainer` \(who deployed the contract\) and opt-in developers. All necessary testing and final verification can be done before making it public. `Contract Maintainer` can also remove the contract at this stage if needed.
* **Deployed Public Contract**: `Contract Maintainer` can make the contract public aka open business to the public.

To incentivize users to be mindful of acquiring storage on a public ledger, and also in effect to reduce scams, we use `State Renting` mechanism. `Contract Maintainer` is required to put up a bond when deploying a contract, which will be refunded when the contract is removed from the chain.

Some barriers of entry would encourage more responsible behaviors, and we are constantly researching the space for better mechanisms to achieve this.

### **6. Compatible Developer Tooling**

Acala EVM enables developers to develop, test and deploy DApps with existing tooling support, such as [Remix](https://remix.ethereum.org/) and [Waffle](https://getwaffle.io/). More toolchains will be added as we progress.

Existing Solidity DApps and node.js applications can communicate with Acala nodes with minimum changes. Developers can use Acala’s web3 provider [bodhi.js](https://github.com/AcalaNetwork/bodhi.js) to interact with an Acala node seamlessly.

### **7. Avoid Dust Accounts**

Dust accounts are accounts with very little funds, generally less than the amount needed to conduct a transaction. Too many dust accounts add unnecessary data to the blockchain, which would make it difficult for full nodes to sync with the network \(since every full node has a complete copy of the blockchain\).

On the Acala network, an address is only active when it holds a minimum amount \(exact number TBD\). This minimum amount is called “Existential Deposit” \(ED\), similar to [ED on Polkadot](https://support.polkadot.network/support/solutions/articles/65000168651-what-is-the-existential-deposit-#:~:text=On%20the%20Polkadot%20network%2C%20an,needed%20to%20conduct%20a%20transaction.). All native tokens e.g. DOT, ACA, aUSD, BTC etc. would have this feature, but it is not enforced upon ERC-20 tokens in the EVM.

