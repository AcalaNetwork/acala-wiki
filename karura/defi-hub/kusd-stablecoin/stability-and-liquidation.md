# Stability & Liquidation

The kUSD is designed to peg to US Dollar at 1:1 ratio through automatic risk management mechanisms and on-chain governance. Behind the scene, the `cdp-engine` is the main module that manages all kUSD vaults, managing surplus and debts, assessing risks, handling liquidation etc to keep the stablecoin protocol solvent and kUSD pegged.

We will outline below the key aspects of risk management and stbailiy mechanisms:

* Autonomous Liquidator
* Liquidation
* Adjusting Risk Parameters
* Auctions & Auction Parameters
* Participate in Auctions
* Oracles
* Emergency Shutdown

