# Single Account

## **Single Wallet, Single Account Experience**

Users can use **one extension/wallet**, and **a single Substrate account** to interact with the Substrate runtime, contracts in EVM, and wasm contracts or a hybrid of these. If a user wants to use a particular Ethereum address, then simply link it with his/her Substrate address \(basically proving the user owns both addresses\), thereafter the user can just use the Substrate account to sign any Ehtereum transactions seamlessly.

This allows users to use all functionalities within Acala and cross-chain capabilities without managing multiple accounts or wallets.

## Using Single Account

A user on Acala will always have a Substrate-based account that enables users to easily navigate multiple blockchains and sign any \(EVM and Susbtrate\) transactions with a single account. Follow the guide [here](https://wiki.acala.network/learn/get-started#create-a-polkadot-account) or [here](https://wiki.polkadot.network/docs/en/learn-account-generation) to generate an account.  To use Acala EVM, you either

1. generate an Ethereum address from the Substrate account OR
2. bind an existing Ethereum address to the Substrate account

### **Generate an EVM Account**

A user can generate an EVM address for each Substrate account. The user then can bind the EVM address to the Substrate account, so balances of native tokens e.g. DOT, renBTC, aUSD etc. on the Substrate account, are then available on the EVM address to use. 

In the Acala EVM, if funds are sent to a Substrate account without an associated EVM address, then an EVM address will be automatically generated and bound with the Substrate account.

Balances are automatically synchronized between the Substrate account and the associated EVM address. For example, a user teleports 10 renBTC to Acala, his/her balance will be shown in the Substrate account, the balance will also be shown and transferrable in the EVM address.

#### EVM Address Generation

The EVM Address is generated using prefix `evm` and the associated Substrate account with `blake2_256` hash function. Check out the source code [here](https://github.com/AcalaNetwork/Acala/blob/master/modules/evm-accounts/src/lib.rs#L185-L186).

```text
blake2_256(“evm:” ++ account_id)[0..20]
```

\[TODO\] step guide to generate and bind an EVM address.

### **Bind an Existing Ethereum Addresses**

In any case, if users want to use an existing Ethereum address in Acala EVM, this address will need to be claimed and bound to their Subatrate account.

One Substrate account can only be associated with one Ethereum address. A Substrate address already linked to a generated EVM address can no longer link to an existing Ethereum address and vice versa.

Binding an existing Ethereum address requires users to prove they own its private key, by signing a `claim` transaction.

Below are two potential use cases of binding an existing Ethereum address.

**Use Case 1**

For example, a DeFi protocol on Ethereum is now expanding its operation to Polkadot, by deploying their contracts on the Acala network. They will this new branch by airdropping tokens to their existing users if they also use the protocols on Acala.

The easiest way is to airdrop tokens to existing Ethereum addresses on Acala. Hence users would just bind their current Ethereum address to a Substrate address, use it for any EVM transactions, and receive airdrops.

**Use Case 2**

For DApps like [Linkdrop](https://linkdrop.io/), users are required to sign messages using Ethereum private key. Using Linkdrop on Acala, would require users to claim their existing Ethereum address, and bind it to their Substrate account. Thereafter they can send transactions on behalf of the Ethereum account.

