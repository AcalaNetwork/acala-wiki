---
description: Liquidity provider is rewarded with exchange fee and an additional reward.
---

# Provide Liquidity

* [Overview](https://wiki.acala.network/learn/basics/deposit-and-earn#overview)
* [Mandala Test Network](https://wiki.acala.network/learn/basics/deposit-and-earn#mandala-test-network)
  * [Via Web DApp](https://wiki.acala.network/learn/basics/deposit-and-earn#via-web-dapp)
    * [Deposit Liquidity](https://wiki.acala.network/learn/basics/deposit-and-earn#deposit-liquidity)
    * [Withdraw Liquidity](https://wiki.acala.network/learn/basics/deposit-and-earn#withdraw-liquidity)
    * [Withdraw Reward](https://wiki.acala.network/learn/basics/deposit-and-earn#withdraw-reward)

## Overview

DeX (Decentralized Exchange) on Acala allows users to quickly swap one token to another and serves three-fold purposes for the platform:

1. **provide liquidity to bootstrap the ecosystem**: there will be markets for bridged in assets like BTC and ETH, as well as DOT, ACA and aUSD.
2. **as a complimentary facility for stablecoin liquidation mechanism to improve stability and reduce risk**: when a liquidation being triggered, the Honzon stablecoin protocol will sell the assets off on the DeX instead of on an auction, if slippage is acceptable.
3. **improve usability**: users can pay fees in the transacting token e.g. aUSD rather than being restricted to the network token ACA

Acala DeX, inspired by Uniswap, uses constant-product mechanism and is implemented as runtime modules hence better integrated with other protocols. Each trading pair using aUSD as base token e.g. BTC/aUSD is represented as an exchange pool. The exchange rate is set by the first liquidity provider by the amount of each token he/she deposits, and will be adjusted over time through arbitrage.

Liquidity provider is rewarded with **exchange fee** and **an additional reward** (from stability fee profit share), as liquidity here not only serve users for token swap, but also serve the Honzon stablecoin protocol for liquidation.

## Mandala Test Network

### Via Web DApp

#### Deposit Liquidity

![Dapp](../../../.gitbook/assets/depositearn\_deposit.png)

#### Withdraw Liquidity

&#x20;Note: withdrawn amount is NOT token amount, but a share amount reflecting your contribution to a particular liquidity pool.

![](../../../.gitbook/assets/depositearn\_withdraw.png)

#### Withdraw Reward

&#x20;These are the additional profit share from stability fee revenue to recognize the contribution of DeX liquidity towards stability of aUSD.

![](../../../.gitbook/assets/depositearn\_reward.png)
