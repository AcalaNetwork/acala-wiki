---
description: Introduction of the Acala modules.
---

# Modules

## Honzon Stablecoin Modules

### [auction\_manager](https://github.com/AcalaNetwork/Acala/tree/master/modules/auction_manager)

`auction_manager` module starts and manages various types of auctions - `surplus auction` when accrued interests exceeds certain limit, `debt auction` when unpaid debt from liquidated positions exceeds certain limit, and `collateral auction` when a position is being liquidated.

**Surplus Auction** When business as usual, the Honzon protocol through the `CDP Treasury` would accrue interests in aUSD from each loan. This surplus will firstly cover any outstanding debt in the system. Whenever this surplus reaches a predefined limit, the surplus in aUSD will be auctioned off for ACA which will then be burnt.

**Debt Auction** The `CDP Treasury` also keeps account of the system debt, that is if unsafe loans cannot be fully liquidated and debt cannot be fully repaid. If system surplus cannot cover the debt, then the `Debt Auction` will be triggered, and additional ACA could be minted to auction off to pay back outstanding aUSD debt.

**Collateral Auction** Each aUSD loan is created with a `required collateral ratio`, and its collateral ratio needs to be above the `liquidation ratio` to stay `safe`. The `liquidation ratio` is lower than the `required collateral ratio` to create a safety zone for each loan and avoid immediate liquidation upon creation. If the price of a collateral drops to a point where a loan's collateral ratio is less than the `liquidation ratio`, then a `Collateral Auction` is triggered to sell off the collaterals to pay off the aUSD debt.

Generally all three types of auctions follow similar formats except `debt auction` has a preset auction end time while the other two are open ended:

* new bid price needs to be &gt;= `old bid` + `target price` \* `minimum increment` \(`minimum increment` as percentage\) e.g. old bid = $5, target price = $100, min. increment = 5%, then new bid price needs to be &gt;= $10
* if bid price exceeds `target price`, a reverse auction starts
* if no more new bids, the auction will last till `the last bid time` + `AuctionTimeToClose` \(in block number\)
* if the auction goes on, and exceeds the `AuctionDurationSoftCap` \(also in block number\), then every new bid's `AuctionTimeToClose` will be halved, `minimum increment` doubled to speed up the auction

### [cdp\_engine](https://github.com/AcalaNetwork/Acala/tree/master/modules/cdp_engine)

`cdp_engine` manages automated liquidation and collateral settlement via off-chain worker, and a set of risk parameters - `stability fee`, `liquidation ratio`, `liquidation penalty`, `required collateral ratio` and `maximum total debit value`.

**`stability fee`** or interest rate charged for aUSD loans are collected every block. aUSD owned plus accumulated stability fee / interest is recorded as `debit units`, the debit exchange rate is updated every block. The [`DebitExchangeRateConvertor`](https://github.com/AcalaNetwork/Acala/blob/master/modules/cdp_engine/src/debit_exchange_rate_convertor.rs) to calculate how much aUSD is owed. Fees collected are added to the `cdp_treasury` surplus pool.

**`Offchain Worker`** is run at the end of every block. Only one `Offchain Worker` is run at a time by acquiring a lock before the job and releasing it after the job.

* Automated liquidation: the `Offchain Worker` would iterate through all loans and liquidate those that are `unsafe`, that is current collateral ratio is below the `liquidation ratio`. It will firstly obtain aUSD directly from the DeX given acceptable slippage, otherwise it will create collateral auction to pay back outstanding debt remain. 
* Collateral settlement: during emergency shutdown, settle all loans by collecting collaterals into `cdp_treasury` and clear debt balance.

### [cdp\_treasury](https://github.com/AcalaNetwork/Acala/tree/master/modules/cdp_treasury)

`cdp_treasury` manages system global `DebitPool` and `SurplusPool`, and surplus, debit and collateral auction parameters.

During auctions, once there's an accepted bid, the amount of surplus, debit or collateral will be transferred to the `cdp_treasury`. If there's a new accepted bid, then it will be refunded to the original account. This ensure security of required funds to the system.

`on_system_surplus` - when there's surplus incurred e.g. stability fees collected, it will be added to the `cdp_treasury`. This surplus would be used to pay for system debt first, but if it exceeds a certain limit, then a `surplus auction` will be triggered. `on_system_debit` - when an aUSD loan is liquidated, the debt is added to `cdp_treasury`.

### [honzon](https://github.com/AcalaNetwork/Acala/blob/master/modules/honzon)

`honzon` is the main interaction point for stablecoin users. It provides the following public functions:

`adjust_loan` - use this to open or update an aUSD loan position. `transfer_loan_from` - use this to transfer the entire balance of a given aUSD loan from one account to another account. The account needs to be `authorized` first. `liquidate_cdp` - this is kinda depreciated as an offchain worker is implemented in `cdp_engine` to handle this automatically. `settle_cdp` - this is kinda depreciated as an offchain worker is implemented in `cdp_engine` to handle this automatically.

### [loans](https://github.com/AcalaNetwork/Acala/blob/master/modules/loans)

`loans` manages all debit balances and collateral balances for all collaterals and accounts in a double map.

`adjust_position` - create or update an aUSD loan position - deposit/withdraw collaterals, borrow/pay back aUSDs, it will validate various conditions and check risk parameters before making adjustment.

`transfer_loan` - transfer the entire balance of a given aUSD loan from one account to another account.

## [emergency\_shutdown](https://github.com/AcalaNetwork/Acala/tree/master/modules/emergency_shutdown)

`emergency_shutdown` is one of the risk management instruments in particular as a last resort to stop and settle the Honzon protocol to protect the assets of both aUSD and loan holders. The shutdown can apply to one or more collateral asset and associated loans.

`emergency_shutdown` - locks price for collaterals, stops opening new loans, stops all auctions, settles outstanding loans, return remaining collaterals to owners.

Note: a collateral asset can be `shut down` individually, not via the `emergency_shutdown` module, but via setting `set_global_params` in the `cdp_engine` module, to cap the `maximum_total_debit_value` \(debt ceiling\) to stop new loans being opened, and raise `liquidation_ratio` gradually to settle outstanding loans.

## Decentralized Exchange

\[TODO\]

## General

### [accounts](https://github.com/AcalaNetwork/Acala/blob/master/modules/accounts)

Acala Network is a multi-token network. The native network token aka ACA is managed by the `balances` pallet. All other tokens are managed by [`tokens` module](https://github.com/laminar-protocol/open-runtime-module-library/tree/master/tokens). The [`currency` module](https://github.com/laminar-protocol/open-runtime-module-library/tree/master/currencies) realizes the multi-token support by combining the two.

`accounts` handles special transfers e.g. free transfers for given conditions, e.g. when certain amount of native token ACA is deposited and locked, then certain number of free transfer transactions are enabled.

### airdrop

\[TODO\]

### [primitives](https://github.com/AcalaNetwork/Acala/tree/master/modules/primitives)

`CurrencyId` lists all supported tokens on the network. `AirDropCurrencyId` lists airdrop tokens, this is available on `Mandala Network` to record canary network and mainnet token airdrop balances. It will be used for token claim once those two later networks are launched.

