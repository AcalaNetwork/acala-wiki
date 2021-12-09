# Liquidation

Liquidating unsafe positions requires selling off some collateral assets to repay kUSD in the vault. The liquidation mechanism uses Karura Swap in combination of collateral auction to ensure efficiency and effectiveness.

The end result of a liquidation is

* the kUSD debt is repaid
* liquidation fee is collected from the vault owner and added to the `cdp_treasury` as surplus
* remaining collaterals are returned to the vault owner
* in an unfortunate liquidation event where not all kUSD can be recopped, `cdp_treasury` will record the bad debt 

![](https://i.imgur.com/i6k6OTz.png)

## Liquidate on Karura Swap

It will calculate the target kUSD amount, which is the sum of the kUSD amount owed and the liquidation penalty needs to be paid. It will then attempt to swap collaterals of the vault for the target kUSD amount on Karura Swap if the slippage is within the accepted level.

If the corresponding kUSD pool on Karura Swap has great liquidity, and the size of the trade is reasonable with regards to the liquidity available, then this will be the most efficient way for liquidaiton.

Otherwise it will create an auction to auction off the collateral for kUSD.

## Liquidate on Collateral Auction

Liquidation auction is used to sell off collaterals to recover kUSD and pay back unsafe vaults, if they cannot be liquidated via Karura Swap. A combination of ascending auction and reverse auction mechanism is used to achieve the desirable outcome. Below is a summary of the process:

1. The liquidated collateral will be auctioned in **an ascending auction** \(where bidders bid upwards\) with a preset kUSD target \(outstanding kUSD debt + liquidation penalty\)
2. Then, once such a bid has been reached, the auction switches to **a descending reserve auction** that allows any potential buyers to bid the minimum amount of the collateralized asset they are willing to accept by paying the amount of the preset kUSD target. Auction ends when no lower bid is placed within the auto extension period.
3. Lastly, the part of collateral sold in the auction mechanism is transferred to the auction winner, and any remaining collateral is returned to the original vault owner. The kUSD debt is now repaid, and the liquidation penalty in kUSD is collected by the `cdp-treasury` as surplus.

### Auction Parameters

These are the important parameters for the collateral auction mechanism, which can also be set and updated via governance.

| Parameters | Type | Description | Definition | Update Method |
| :--- | :--- | :--- | :--- | :--- |
| MinimumIncrementSize | Rate | The minimum increment size of each bid compared to the previous one. | `cdpEngine` constant | config on runtime |
| AuctionTimeToClose | BlockNumber | The extended time for the auction to end after each successful bid. | `auctionManager` constant | config on runtime |
| AuctionDurationSoftCap | BlockNumber | When the total duration of the auction exceeds this soft cap, push the auction to end more faster. | `auctionManager` constant | config on runtime |
| ExpectedCollateralAuctionSize | Balance | The expected amount size for per lot collateral auction of specific collateral type. | `cdpTreasury` storage | update call by `UpdateOrigin` of `cdpTreasury` |
| MaxAuctionsCount | u32 | The cap of lots number when create collateral auction on a liquidation or to create debit/surplus auction on block end. | `cdpTreasury` constant | config on runtime |

### Current Values

| Parameter | Value |
| :--- | :--- |
| MinimumIncrementSize | 2 % from auction value |
| AuctionTimeToClose | 75 blocks \(~15 min\) |
| AuctionDurationSoftCap | 600 blocks \(~2h\) |
| ExpectedCollateralAuctionSize | 150 KSM / 1500 LKSM |
| MaxAuctionsCount | 100 |

