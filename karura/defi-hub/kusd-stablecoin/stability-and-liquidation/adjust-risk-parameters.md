# Adjust Risk Parameters

These are the most important risk parameters used to maintain the stability and security of the protocol. These parameters are set and updated for each available collateral asset via Karura on-chain Governance.

| Parameters | **Type** | **Description** |
| :--- | :--- | :--- |
| **StabilityFee** | Rate | Cost for minting kUSD, used to adjust supply and demand of kUSD |
| **LiquidationRatio** | Ratio | Price volatility affects collateral risk profiles. A more risky collateral asset is usually associated with a higher LiquidationRatio, and vice versa |
| **LiquidationPenalty** | Rate | Cost for allowing your vault to be liquidated,  a disincentive for leaving a vault unsafe |
| **RequiredCollateralRatio** | Ratio | Required when minting kUSD, usually higher than LiquidationRatio as a safety cushion  |
| **MaximumTotalDebitValue** | Balance | Debt Ceiling for a particular collateral, used to control portfolio diversification, risks willing to take on |
| **MaxSlippageSwapWithDEX** | Ratio | Acceptable slippage when liquidating on Karura Swap, to ensure price efficiency |

Read more on what these parameters mean for kUSD [here](../mint-kusd.md#key-parameters).

**Gaunlet Risk Assessment & Parameter Recommendation for KSM** [**here**](https://medium.com/gauntlet-networks/karura-parameter-recommendation-methodology-6ce7fe06cb77)**.**

