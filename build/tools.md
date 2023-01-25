# Tools

## Block Explorers

* [Polkadot Explorer](https://polkadot.js.org/apps/#/explorer) - Acala console dashboard block explorer. Currently connects to Acala by default, but can be configured to connect to other remote or local endpoints.
* [Polkascan](https://polkascan.io/) - Blockchain explorer for Polkadot, Kusama, and other related chains.
* [Subscan](https://subscan.io/) - Blockchain explorer for Substrate chains, include Acala.

## Wallets

> The integration of a wallet with Acala allows for simple and easy access to private keys and signing transactions. Below are some wallets that support Acala along with their development statuses. Note that inclusion does not necessarily imply endorsement of that wallet.

### Supported Wallets

| Wallet Name                                                    | Development State                                                                                                                            | Team Name   | Description                         |
| -------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | ----------------------------------- |
| [Acala DApp](https://apps.acala.network/)                      | Live                                                                                                                                         | Acala       | Browser                             |
| [Polkawallet](https://polkawallet.io/)                         | Live                                                                                                                                         | Polkawallet | IOS and Android                     |
| [Signer](https://www.parity.io/signer/)                        | Building                                                                                                                                     | Parity      | IOS and Android                     |
| [Polkadot-JS](https://polkadot.js.org/apps/#/accounts)         | Live                                                                                                                                         | Parity      | Browser                             |
| [Talisman](https://talisman.xyz/)                              | Live                                                                                                                                         | Talisman    | Browser extension & Browser         |
| [SubWallet](https://subwallet.app/)                            | Live [\[for Acala\]](https://docs.subwallet.app/dapps-user-guide/acala) [\[for Karura\]](https://docs.subwallet.app/dapps-user-guide/karura) | SubWallet   | Browser extension & IOS and Android |
| [Enkrypt](https://enkrypt.com/)                      | Live                                                                                                                                         | MyEtherWallet       | Browser extension & Browser           |
| [Polkadot.JS Plugin](https://github.com/polkadot-js/extension) | Building                                                                                                                                     | Parity      | Browser extension                   |

## Network Monitoring & Reporting

* [Polkadot Telemetry Service](https://telemetry.polkadot.io/) - Network information including what nodes are running the chain, what software versions they are running, sync status, and location.

## Libraries

* [acala.js](https://github.com/AcalaNetwork/acala.js) - This SDK is an extension of [polkadot.js](https://github.com/polkadot-js/api), with additional type and metadata information, makes it easy for users to interact with the Acala Network.

## Data Analysis

* [@acala-weaver/graphql-server ](https://github.com/AcalaNetwork/chain-sync-server/tree/master/packages/graphql-server)- support a query server base on graphQL.
* [@acala-weaver/chain-recoder](https://github.com/AcalaNetwork/chain-sync-server/tree/master/packages/chain-recoder) - sync chain and maintain accounts, CDP and so on.
* [@acala-weaver/storage-recoder](https://github.com/AcalaNetwork/chain-sync-server/tree/master/packages/storage-recoder) - sync chain storage data to a time series database for monitor and alert.

## Faucet

* [Faucet-bot](https://github.com/AcalaNetwork/faucet-bot) - the code for Riot and Discord faucet bot.
* [Discord Faucet Channel ](https://www.acala.gg/)- Discord faucet channel.

## Collateral Auction & Liquidation Bot

We have currently 2 bots for monitoring of the liquidation auctions.

* [Collateral Auction Bot for Acala/Karura](https://github.com/open-web3-stack/guardian/tree/master/packages/example-guardian#collateral-auction-bot-for-acalakarura) - This example uses the collateral auction. It will watch collateral auctions and DEX pools, and bid from the specified account if the price of collateral is lower than the price from Price oracle for MARGIN coefficient.
* [Collateral Auction Bot Example](https://github.com/AcalaNetwork/collateral-auction-bot-example) - This is an example Auction bot that places bids for ongoing liquidation of unsafe vaults for vaults with LKSM as collateral.

## EVM Tools

* [EVM Playground](https://evm.acala.network) - a web application to test, deploy and execute smart contracts on Acala EVM
