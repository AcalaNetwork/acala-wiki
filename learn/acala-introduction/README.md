---
description: A relatively comprehensive introduction.
---

# Acala Introduction

Acala is a specialized stablecoin and liquidity blockchain that is decentralized, cross-chain by design, and future-proof with forkless upgradability. Acala stablecoin protocol uses multi-collateral-backing mechanism to create a stablecoin soft-pegged to the US Dollar. The Acala stablecoin protocol is essentially a decentralized monetary reserve that creates a stable currency from a basket of reserve assets, where holders of the reserve assets can spend, trade, access other services without price volatility while retaining their exposure to (without selling) the reserve assets.

Acala powered by Polkadot is cross-chain by nature. The reserve assets can be Polkadot native asset (DOT and DOT-derivatives), Acala native asset (ACA), and cross-chain assets like Bitcoin (BTC) and Ether (ETH). Acala stablecoin can be trustlessly integrated by other blockchains connected to Polkadot as well as applications on those chains. The Acala ecosystem continues to expand with a network of decentralized protocols, applications and parachains.

## Assets

* **Acala Dollar (aUSD)**: the Acala stablecoin that is soft-pegged to (stabilized against) the US Dollar. Minting Acala Dollar is trustless, sovereign and liberating act of the minting, using reserve assets in excess of value as collateral.&#x20;
* **Reserve Assets**: DOT, LDOT ([Liquid Staking DOT powered by Homa Protocol](https://docs.homastaking.app/)) and ACA are default reserve assets. New cross-chain assets can be included as reserve assets upon assessment by the Financial Committee and Risk Assessor like [Gauntlet](https://gauntlet.network/).&#x20;
* **Acala Token (ACA)**: the native token of Acala network. There is a fixed supply of ACA. ACA is the reserve asset to generate Acala stablecoin, and the governance token to regulate stablecoin price by adjusting risk parameters such as interest rate. ACA is also the transaction fees token of the network.&#x20;

## Why Acala Stablecoin

Stablecoin since its inception to widespread popularity has proven its utility beyond speculation: they have been used in alleviating economic and political hardship, as a hedging mechanism for traders, and to release liquidity of underlying crypto assets without foregoing ownership of those assets.

Users can get Acala stablecoin via [AcalaSwap](https://docs.acalaswap.app/) or generate Acala Stablecoin by depositing collaterals to a stablecoin vault.&#x20;

These are some of the use cases for Acala Stablecoin:

* Minting Acala stablecoin is **trustless, sovereign and liberating** act of the minter
* Without foregoing ownership of the underlying asset, business operators may collateralize them to mint Acala stablecoin to **hedge volatility, pay for goods and services**
* Traders may **leveraged long the underlying asset** by depositing the underlying asset to stablecoin vault, minting stablecoin, and swap more underlying assets, essentially multiplying exposure of underlying asset
* Traders can use yield-bearing assets such as LDOT ([Liquid Staking DOT powered by Homa Protocol](https://docs.homastaking.app/)) as collateral to **multiply exposure of the underlying asset while earning APR**
* Liquidity Providers to can **increase capital efficiency** when providing liquidity to pools by using LP collateralized super loans
* Acala stablecoin is designed to be stable, and an **ideal choice for medium of exchange**. Users can use it to pay transaction fees, trade, for remittance, on-and-off ramps as it integrates with more partners. Projects can use it as a quote asset for listing.
* Acala stablecoin **is a DeFi building block in a multi-chain ecosystem**, it will empower more ecosystem growth and new product innovations

## Stablecoin Mechanisms

Acala stablecoin protocol employs a dynamic system of Collateralized Debt Position (CDPs), on-chain liquidator, Oracle Quality of Service, market arbitrage incentives and risk management mechanisms to create a stablecoin that is robustly tracking the dollar in varied market conditions.

We will outline below the key aspects of risk management and stability mechanisms:

* Risk Parameter Mechanism
* Autonomous Liquidator
* Liquidation Event
* Oracles
* Emergency Shutdown

### Risk Parameter Mechanism

Multiple types of reserve assets with different risk profiles are accepted as collaterals of CDPs,  each of which has its own set of risk parameters to create a diversified basket of assets backing the stablecoin. The protocol dynamically manages the debt positions and safeguard the solvency of the protocol using these risk parameters.

* **Stability Fee:** this is the interest rate or cost for minting Acala stablecoin. It is used as incentive to influence supply and demand of the stablecoin
* **Liquidation Ratio:** this is the ratio at which the collaterals will be sold to pay off outstanding stablecoin debt to ensure solvency of the system. Price volatility affects collateral risk profiles. A more risky collateral asset is usually associated with a higher LiquidationRatio, and vice versa
* **Liquidation Penalty:** cost for allowing your vault (loan) to be liquidated, a disincentive for leaving a vault unsafe
* **Required Collateral Ratio:** required when minting stablecoin. This is usually higher than the LiquidationRatio as a safety cushion to prevent immediate liquidation after minting for volatile assets
* **Debt Ceiling:** debt Ceiling for a particular collateral, used to control portfolio diversification, risks willing to take on
* **Accepted Slippage:** the system liquidates on AcalaSwap when slippage is within acceptable levels before goes to collateral auction, to ensure price efficiency

### Autonomous Liquidator

Unlike existing implementation of loan liquidators (or more widely known as Keepers in DeFi), Acala stablecoin protocol creates an on-chain autonomous liquidator that checks vault safety every block and triggers liquidation process for unsafe vaults. The autonomous Liquidator&#x20;

* is **decentralized**, **more secure and efficient** mechanism to safeguard the protocol&#x20;
* compared to the traditional method of relying on external parties to liquidate (e.g. off-chain Keeper servers)
* its code is **an integral part of the blockchain runtime** and **guaranteed by the network consensus**
* does not take up on-chain resources for execution, hence **highly secure, scalable and efficient**.&#x20;

A vault is safe, if

$$
Current~Ratio > Liquidation~Ratio
$$

The `current ratio` is the debt to collateral ratio based on the current market value of the collateral asset, given by&#x20;

$$
Current~Rratio = minted~aUSD / Collateral~Market~ Price~($)
$$

Once the autonomous Liquidator identifies an unsafe vault, it will trigger the liquidation process to sell off the collateral for Acala stablecoin to repay the vault. The liquidation process uses a hybrid mechanism of liquidating on decentralized exchange and collateral auctions.

\[[Source](https://github.com/AcalaNetwork/Acala/blob/master/modules/cdp-engine/src/lib.rs#L488)]

### Liquidation Process

Liquidating unsafe positions requires selling off some collateral assets to repay Acala stablecoin owed (borrowed from the vault). The liquidation mechanism uses [AcalaSwap](https://docs.acalaswap.app/) in conjunction of Collateral Auction to ensure efficiency and effectiveness.

The end result of a liquidation is

* the Acala stablecoin debt is repaid (the protocol is solvent)
* liquidation fee is collected from the vault owner and added to the `cdp_treasury` as surplus
* remaining collaterals are returned to the vault owner
* in extreme market conditions, if collaterals cannot be sold (either via DeX or Auction) to repay debt and liquidation penalty, then the collateral will be collected by the `cdp_treasury`, while the outstanding aUSD will be recorded as debt. These collaterals can be sold at a later time (managed by governance) to repay the outstanding aUSD.
* in an unfortunate liquidation event where not all Acala stablecoin can be recouped, `cdp_treasury` will record it as bad debt

![](<../../.gitbook/assets/Screen Shot 2022-02-15 at 9.43.41 AM.png>)

#### Liquidation on AcalaSwap

* the protocol calculates target Acala stablecoin amount (owed + liquidation penalty) to be purchased on the swap
* the protocol swaps collateral asset from the vault for target Acala stablecoin if the slippage is within a predefined acceptable level

This is a far more efficient way for liquidation if the size of the trade is reasonable, and the swap has reasonable liquidity.&#x20;

#### Liquidation on Collateral Auction

The Collateral Auction employs a combination of ascending auction and reverse auction mechanism to achieve the desirable outcome - get the best stablecoin price for the collaterals. Below is a summary of the process:

1. The liquidated collateral will be auctioned in **an ascending auction** (where bidders bid upwards) with a preset stablecoin target (outstanding debt + liquidation penalty)
2. Then, once such a bid has been reached, the auction switches to **a descending reserve auction** that allows any potential buyers to bid the minimum amount of the collateralized asset they are willing to accept by paying the amount of the preset stablecoin target. Auction ends when no lower bid is placed within the auto extension period.
3. Lastly, the part of collateral sold in the auction mechanism is transferred to the auction winner, and any remaining collateral is returned to the original vault owner. The stablecoin debt is now repaid, and the liquidation penalty in stablecoin is collected by the `cdp-treasury` as surplus.

### Auction Parameters

These are the important parameters for the collateral auction mechanism, which can also be set and updated via governance.

| Parameters                    | Type        | Description                                                                                                             | Definition                | Update Method                                  |
| ----------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------- | ---------------------------------------------- |
| MinimumIncrementSize          | Rate        | The minimum increment size of each bid compared to the previous one.                                                    | `cdpEngine` constant      | config on runtime                              |
| AuctionTimeToClose            | BlockNumber | The extended time for the auction to end after each successful bid.                                                     | `auctionManager` constant | config on runtime                              |
| AuctionDurationSoftCap        | BlockNumber | When the total duration of the auction exceeds this soft cap, push the auction to end more faster.                      | `auctionManager` constant | config on runtime                              |
| ExpectedCollateralAuctionSize | Balance     | The expected amount size for per lot collateral auction of specific collateral type.                                    | `cdpTreasury` storage     | update call by `UpdateOrigin` of `cdpTreasury` |
| MaxAuctionsCount              | u32         | The cap of lots number when create collateral auction on a liquidation or to create debit/surplus auction on block end. | `cdpTreasury` constant    | config on runtime                              |

#### Arbitrage Opportunity

This then poses great arbitrage opportunity to purchase potentially discounted collateral assets via the Collateral Auction, or arbitrage on the collateralAsset-stablecoin swap pair after a large liquidation. This market incentivized mechanism will help safeguard the protocol and bring the price back to equilibrium.

## Open Oracle Gateway

The challenge of providing reliable, accurate and decentralized oracles cannot be solved by Acala alone. When taking into account that Acala is the DeFi hub and platform powering more cross-chain DeFi DApps on Polkadot, Kusama and beyond, creating a more open, inclusive, and decentralized oracle infrastructure with other leading projects in the industry becomes critical. The Open Oracle Gateway (OOG) is a significant step towards that vision.&#x20;

The Gateway allows multiple oracles to be deployed on the Acala network, leveraging Acala's DeFi optimized oracle infrastructure, and serving any DApps on Acala, Polkadot, Kusama and beyond.  Specifically, the Gateway offers:

1. **Multiple Oracle Networks**: multiple parties in addition to Acala Oracle can operate their own oracle services. Providers can set up their own oracle network with node operators, or integrate with an oracle API.&#x20;
2. **Price Feeds by Choice**: Dapps can choose to use an aggregated feed from all providers or a single provider, or they can obtain raw data from individual node operators and aggregate them themselves.&#x20;
3. **Quality of Service**: All price feeds posted through the Gateway will be provided with Quality of Service - these transactions are _operational transactions_ (system critical transactions) that are prioritized and guaranteed to be included in a block.
4. **FREE of Fees**: All valid feeds will be refunded with the transaction fees incurred, essentially making oracle feeds FREE for providers while preventing spam and ensuring integrity.&#x20;
5. **Progressive Decentralization**: Acala network will progressively decentralize, start with PoA (appointed Council governance), then evolve into elected Council governance and eventually democracy. The Gateway then initially will require sitting Council approval for accepting new oracle providers and their node operators.&#x20;

### Acala Stablecoin Oracle Feeds

The Acala Oracle powered by the Open Oracle Framework is used to feed real-time market price information to the stablecoin protocol.&#x20;

* The Acala Oracle operates a network of oracle node operators, it aggregates a number of external price feeds and provides a medianized reference price
* The adding and removal of whitelisted operators is managed via governance
* All price feeds will be provided with Quality of Service - these transactions are prioritized and guaranteed to be included in a block
* All valid feeds will be refunded with the transaction fees incurred, essentially making oracle feeds free for providers while preventing spam and ensuring integrity.&#x20;

Check price feeds for

* [Acala Network](https://apps.acala.network/vault/oracle)
* [Karura Network](https://apps.karura.network/vault/oracle)

\[price [Source](https://github.com/AcalaNetwork/Acala/tree/master/modules/prices)]\
\[oracle [Source](https://github.com/open-web3-stack/open-runtime-module-library/tree/8895f4b4c6ce9b9f03652bfe9602f102c7caa937/oracle)]

### Emergency Shutdown

Emergency shutdown is the last resort to protect the stablecoin system against serious security threats. The shutdown can be **isolated to a particular collateral or applied as global shutdown**. It is initiated via governance. It will enforce target prices of assets, and ensure Acala stablecoin and vault owners receive the value of assets they are entitled to.

When an Emergency Shutdown is initiated, the following would happen:

1. **Shutdown triggered**: users are not allowed to update stablecoin positions, the system will lock a target price for collateral assets, however users are free to withdraw any free collaterals i.e. collaterals that have not been used to mint Acala stablecoin.
2. **Vault processing**: the system will stop and clear all relevant auctions, process outstanding vaults and debts. This may take some time, but ultimately collateral assets will be available for Acala stablecoin holders to reclaim
3. **Reclaim debts**: upon step #2 completion, the system will return to the initial status of zero surplus and zero debt, users can burn their Acala stablecoin and reclaim equivalent value of collateral assets.

\[emergency-shutdown [Source](https://github.com/AcalaNetwork/Acala/tree/master/modules/emergency-shutdown)]

## Network Security

Acala blockchain is launched as a parachain of Polkadot essentially using Polkadot's network security and consensus for its block validation and finalization. Acala inherits trustless bridge between itself and Polkadot as well as all connected parachains using cross-chain message passing (or [XCM](https://wiki.polkadot.network/docs/learn-crosschain)).

### Collators

Acala blockchain is maintained by collators, who maintains a full node, collects parachain transactions from users and producing state transition proofs for Polkadot validators. In other words, collators maintain parachains by aggregating parachain transactions into parachain block candidates and producing state transition proofs for validators based on those blocks.

Acala blockchain is secured by the Polkadot Relay Chain, and kept alive by the collators. **Unlike validators, collators has nothing to do with security of the network, by being a parachain, the network is by default trustless and decentralized, and a parachain only needs one honest collator to be censorship-resistant.** Read more on Collators [here](https://wiki.polkadot.network/docs/learn-collator).

### Parachain Slots

Polkadot has a limited number of parachains. It uses an auction mechanism to allocate parachain slots where the DOT tokens are used for bidding. A parachain can lease the slot for up to 2 years on Polkadot (1 year on Kusama). Acala has been building an on-chain Treasury of DOTs (network-controlled-value) to ensure it is self-sustainable. This is also the magic behind ACA being a fixed supply non-inflationary network token.

* [Polkadot Parachain](https://wiki.polkadot.network/docs/learn-parachains)
* [Treasury Whitepaper](https://github.com/AcalaNetwork/Acala-white-paper/blob/master/Building\_a\_Decentralized\_Sovereign\_Wealth\_Fund.pdf)
* [Acala Whitepaper](https://github.com/AcalaNetwork/Acala-white-paper/blob/master/Acala\_Whitepaper.pdf)
* [Acala Token Economy Paper](https://github.com/AcalaNetwork/Acala-white-paper/blob/master/Acala\_Token\_Economy\_Paper.pdf)
