# Open-Web3-Stack & ORML

Open Web3 Stack is a common-good collection of libraries to accelerate application development on Substrate. It aims to provide application building blocks that are common for most specialist chains.

Open Web3 Stack contains the following repos

* **Open Runtime Module Library (ORML)** where we implemented all the commonly used pallets, modules
* **Open-web3.js** - frontend SDK for using extended Substrate logic from ORML
* **Guardian** - a worker for monitoring and executing certain tasks
* **Rococo-community**: a hosted environment for testing parachains and cross-chain communication

### ORML

Find out more [here](https://github.com/open-web3-stack/open-runtime-module-library).

* [orml-traits](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/traits)
  * Shared traits including `BasicCurrency`, `MultiCurrency`, `Auction` and more.
* [orml-utilities](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/utilities)
  * Various utilities including `OrderSet`.
* [orml-tokens](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/tokens)
  * Fungible tokens module that implements `MultiCurrency` trait.
* [orml-currencies](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/currencies)
  * Provide `MultiCurrency` implementation using `pallet-balances` and `orml-tokens` module.
* [orml-nft](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/nft)
  * Non-fungible-token module provides basic functions to create and manager NFT(non fungible token)
* [orml-oracle](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/oracle)
  * Oracle module that makes off-chain data available on-chain.
* [orml-auction](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/auction)
  * Auction module that implements `Auction` trait.
* [orml-vesting](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/vesting)
  * Provides scheduled balance locking mechanism, in a _graded vesting_ way.
* [orml-gradually-update](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/gradually-update)
  * Provides way to adjust numeric parameter gradually over a period of time.
* [orml-xtokens](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/xtokens)
  * Provides way to do cross-chain assets transfer.
  * [Step-by-Step guide](https://github.com/open-web3-stack/open-runtime-module-library/wiki/xtokens) to make XCM cross-chain fungible asset transfer available on your parachain
* [orml-xcm-support](https://github.com/open-web3-stack/open-runtime-module-library/blob/master/xcm-support)
  * Provides traits, types, and implementations to support XCM integration.

### Guardian

With Guardian, **with mere configuration**, you can set up a number of automatic tasks for monitoring and executing commands for a chain of concern. A task can be `monitoring margin positions` with conditions (if collateral ratio < 110%) then trigger actions (e.g. post warning message to database service, or execute a script to add position).

Here're the [examples](https://github.com/open-web3-stack/guardian/tree/master/packages/example-guardian) and relevant [documentation](https://github.com/open-web3-stack/guardian/tree/master/packages/guardian/docs).

### Open-web3.js

Open-web3 is a bunch of frontend packages that allow to interact with orml, indexer and oracles. Find out more [here](https://github.com/open-web3-stack/open-web3.js).

### Testnet

The TL;DR is, we recommend to use [Chopsticks](https://github.com/AcalaNetwork/chopsticks) and create your own testnet.

Chopsticks is a testing client that can fork a Substrate network with ease, with Chopsticks, you can fork Acala (or even two parachains on both sides of a cross-chain transfer) at a specific block height and start interact/test with it. As the 'testnet' is forked when needed, it will have the latest runtime and latest code, you can test with confident that your code will behave the same in production if it is going live now.

Get started [here](https://github.com/AcalaNetwork/chopsticks).

P.S. In case you are looking for it, Rococo parachain testnet is retired.
