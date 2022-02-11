# Acala Account

### Acala Account

Acala is a Substrate-based blockchain and uses Substrate-based accounts. Users can use one account aka. one private-public keypair on multiple Substrate-based chains. Essentially for one private-public keypair \(one account\) there is a different address representation for different blockchains.

* Generic Substrate addresses start with 5. E.g. the addresses in the Polkadot{js} extension
* You can check your address representation on different blockchain [here](https://acala-testnet.subscan.io/tools/ss58_transform)

The address format used in Substrate-based chains is SS58. SS58 is a modification of Base-58-check from Bitcoin with some minor modifications. Find out more on the Substrate address format [here](https://wiki.polkadot.network/docs/en/learn-accounts). 

Follow the guide [here](https://wiki.acala.network/learn/get-started#create-a-polkadot-account) or [here](https://wiki.polkadot.network/docs/en/learn-account-generation) to generate a Substrate account. 

### EVM Account

Ethereum has a different address format than Substrate. When using smart contract DApps deployed in EVM, users will normally need to use an Ethereum address to transact. On Acala EVM, we have the Single Account feature that allows users to bind their Substrate account with an EVM address once, thereafter they can use the Substrate account to sign any transactions on Acala. 

Read more [here](https://wiki.acala.network/learn/basics/acala-evm/acala-evm-composable-defi-stack/single-account).



