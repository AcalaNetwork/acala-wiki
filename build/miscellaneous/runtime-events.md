---
description: Acala Runtime Events
---

# Runtime Events

## Auction Related

| Event | Parameters | Description | Module |
| :--- | :--- | :--- | :--- |
| NewCollateralAuction | 1. `AuctionId` : new collateral auction id&lt;/br&gt; 2. `CurrencyId` : collateral type&lt;/br&gt; 3. `Balance` : collateral amount&lt;/br&gt; 4. `Balance` : target aUSD amount, when bid &gt;= target, auction is in reverse stage&lt;/br&gt; | collateral auction created | AuctionManager |
| NewDebitAuction | 1. `AuctionId` : auction id&lt;/br&gt; 2. `Balance` : initial avalible ACA amount &lt;/br&gt; 3. `Balance` : fixed aUSD amount, bid must &gt;= it&lt;/br&gt; | debit auction created | AuctionManager |
| NewSurplusAuction | 1. `AuctionId` : auction id&lt;/br&gt; 2. `Balance` : aUSD amount of surplus&lt;/br&gt; | surplus auction created | AuctionManager |
| CancelAuction | 1. `AuctionId` : auction id&lt;/br&gt; | auction canceled | AuctionManager |
| AuctionDealed | 1. `AuctionId` : auction id&lt;/br&gt; | auction dealed | AuctionManager |
| Bid | 1. `AuctionId` : auction id&lt;/br&gt; 2. `AccountId` : bidder address &lt;/br&gt; 3. `Balance` : bid price&lt;/br&gt; | bid succeeded | Auction |

## DEX Related

| Event | Parameters | Description | Module |
| :--- | :--- | :--- | :--- |
| AddLiquidity | 1. `AccountId` : who add liquidity to pool&lt;/br&gt; 2. `CurrencyId` : specific liquidity pool of **other token** / aUSD pair&lt;/br&gt; 3. `Balance` : other token amount added&lt;/br&gt; 4. `Balance` : aUSD amount added&lt;/br&gt; 5. `Share` : increased share amount&lt;/br&gt; | add liquidity to specific pool | Dex |
| WithdrawLiquidity | 1. `AccountId` : who withdraw liquidity from pool&lt;/br&gt; 2. `CurrencyId` : specific liquidity pool of **other token** / aUSD pair&lt;/br&gt; 3. `Balance` : other token amount withdrew &lt;/br&gt; 4. `Balance` : aUSD amount withdrew&lt;/br&gt; 5. `Share` : decreased share amount&lt;/br&gt; | withdraw liquidity from specific pool | Dex |
| Swap | 1. `AccountId` : exchanger&lt;/br&gt; 2. `CurrencyId` : the token type which exchanger sold to DEX &lt;/br&gt; 3. `Balance` : amount of sold token &lt;/br&gt; 4. `CurrencyId` : the token type which exchanger bought from DEX&lt;/br&gt; 5. `Balance` : amount of bought token&lt;/br&gt; | swap token to another token with DEX | Dex |

## Homa Related

| Event | Parameters | Description | Module |
| :--- | :--- | :--- | :--- |
| BondAndMint | 1. `AccountId` : caller&lt;/br&gt; 2. `Balance` : DOT amount to bond by homa protocol &lt;/br&gt; 3. `Balance` : issued LDOT amount to caller&lt;/br&gt; | lock DOT to Homa protocal and issue liquid DOT | StakingPool |
| RedeemByUnbond | 1. `AccountId` : caller&lt;/br&gt; 2. `Balance` : amount of LDOT to redeem&lt;/br&gt; | redeem DOT by normal unbonding | StakingPool |
| RedeemByFreeUnbonded | 1. `AccountId` : caller&lt;/br&gt; 2. `Balance` : fee in LDOT&lt;/br&gt; 3. `Balance` : LDOT amount to redeem&lt;/br&gt; 4. `Balance` : redeemed DOT amount&lt;/br&gt; | redeem DOT directly with Homa free pool | StakingPool |
| RedeemByClaimUnbonding | 1. `AccountId` : caller&lt;/br&gt; 2. `EraIndex`: target era index to claim redeption&lt;/br&gt; 3. `Balance` : fee in LDOT&lt;/br&gt; 4. `Balance` : LDOT amount to redeem&lt;/br&gt; 5. `Balance` : redeemed DOT amount&lt;/br&gt; | redeem DOT by claiming unbonding | StakingPool |

## CDP Related

| Event | Parameters | Description | Module |
| :--- | :--- | :--- | :--- |
| Authorization | 1. `AccountId` : authorizer account&lt;/br&gt; 2. `AccountId` : the authorized account&lt;/br&gt; 3. `CurrencyId` : the authorized loan type&lt;/br&gt; | An account authorize other account to manipulate its specific type of loan | Honzon |
| UnAuthorization | 1. `AccountId` : authorizer account&lt;/br&gt; 2. `AccountId` : the authorized account&lt;/br&gt; 3. `CurrencyId` : the authorized loan type&lt;/br&gt; | authorizer account revoke the right of authorized account to manipulate specific type of loan | Honzon |
| UnAuthorizationAll | 1. `AccountId` : authorizer account&lt;/br&gt; | account revoked all authorization to other accounts with all loan types | Honzon |
| LiquidateUnsafeCDP | 1. `CurrencyId` : collateral token type&lt;/br&gt; 2. `AccountId` : owner of the liquidated CDP&lt;/br&gt; 3. `Balance` : the confiscated amount for liquidation&lt;/br&gt; 4. `Balance` : the bad debt amount\(in aUSD\) produced by the liquidated Cdp&lt;/br&gt; | liquidate an unsafe CDP | CdpEngine |
| SettleCDPInDebit | 1. `CurrencyId` : collateral token type&lt;/br&gt; 2. `AccountId` : owner of the settled CDP&lt;/br&gt; | settle a CDP has debit after emergency shutdown begin | CdpEngine |
| UpdatePosition | 1. `AccountId` : CDP's owner&lt;/br&gt; 2. `CurrencyId` : CDP's collateral type&lt;/br&gt; 3. `Amount` : collateral adjustment amount, positive means collateral increased, negative means collateral decreased&lt;/br&gt; 4. `DebitAmount` : debit adjustment amount, positive means CDP's debit increased and owner get more aUSD loan, negative means CDP's debit decreased and owner repay some debt&lt;/br&gt; | cdp owner manipulate his loan and collateral | Loans |
| ConfiscateCollateralAndDebit | 1. `AccountId` : CDP's owner&lt;/br&gt; 2. `CurrencyId` : CDP's collateral type&lt;/br&gt; 3. `Balance` : collateral amount to be confiscated &lt;/br&gt; 4. `DebitBalance` : debit balance to be confiscated &lt;/br&gt; | emit by system operations for CDPs, such as settlement, liquidation | Loans |
| TransferLoan | 1. `AccountId` : sender&lt;/br&gt; 2. `AccountId` : receiver&lt;/br&gt; 3. `CurrencyId` : collateral type&lt;/br&gt; | sender transfer his whole specific collateral CDP\(include all collateral amount and debit amount\) to receiver | Loans |

## Emergency Shutdown Related

| Event | Parameters | Description | Module |
| :--- | :--- | :--- | :--- |
| Shutdown | 1. `BlockNumber` : block number the emergency shutdown occurs | system emergency shutdown | EmergencyShutdown |
| OpenRefund | 1. `BlockNumber` : block number when refund operation is allowed | refund operation opened | EmergencyShutdown |
| Refund | 1. `Balance` : aUSD amount used to refund | refund succeeded | EmergencyShutdown |

## Risk Management Parameters Related

| Event | Parameters | Description | Module |
| :--- | :--- | :--- | :--- |
| UpdateStabilityFee | 1. `CurrencyId` : collateral type&lt;/br&gt; 2. `Option<Rate>` : extra stalibity fee rate | param updated | CdpEngine |
| UpdateLiquidationRatio | 1. `CurrencyId` : collateral type&lt;/br&gt; 2. `Option<Ratio>` : liquidation ratio | param updated | CdpEngine |
| UpdateLiquidationPenalty | 1. `CurrencyId` : collateral type&lt;/br&gt; 2. `Option<Rate>` : extra penalty rate when CDP liquidated | param updated | CdpEngine |
| UpdateRequiredCollateralRatio | 1. `CurrencyId` : collateral type&lt;/br&gt; 2. `Option<Ratio>` : the limit of collateral ratio when owner manipulate | param updated | CdpEngine |
| UpdateMaximumTotalDebitValue | 1. `CurrencyId` : collateral type&lt;/br&gt; 2. `Balance` : aUSD cap issued by this type of collateral | param updated | CdpEngine |
| UpdateSurplusAuctionFixedSize | 1. `Balance` : fixed aUSD amount to be sold in a surplus auction&lt;/br&gt; | param updated | CdpTreasury |
| UpdateSurplusBufferSize | 1. `Balance` : surplus pool buffer size, surplus auction is only created when surplus pool exceed it&lt;/br&gt; | param updated | CdpTreasury |
| UpdateInitialAmountPerDebitAuction | 1. `Balance` : initial ACA amount to be sold in a debit auction&lt;/br&gt; | param updated | CdpTreasury |
| UpdateDebitAuctionFixedSize | 1. `Balance` : fixed aUSD amount to buy a debit auction&lt;/br&gt; | param updated | CdpTreasury |
| UpdateCollateralAuctionMaximumSize | 1. `CurrencyId` : collateral type&lt;/br&gt; 2. `Balance` : max collateral amount to be sold when create new collateral auction | param updated | CdpTreasury |

