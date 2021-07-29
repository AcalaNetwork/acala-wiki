# Emergency Shutdown

Emergency shutdown is the last resort to protect the stablecoin system against serious security threats. The shutdown can be **isolated to a particular collateral or applied as global shutdown**. It is initiated via [Karura Governance](../../../get-started/governance/). It will enforce target prices of assets, and ensure kUSD and vault owners receive the value of assets they are entitled to.

When an Emergency Shutdown is initiated, the following would happen:

1. **Shutdown triggered**: users are not allowed to update kUSD positions, the system will lock a target price for collateral assets, however users are free to withdraw any free collaterals i.e. collaterals that have not been used to mint kUSD.
2. **Vault processing**: the system will stop and clear all relevant auctions, process outstanding vaults and debts. This may take some time, but ultimately collateral assets will be available for kUSD holders to reclaim
3. **Reclaim debts**: upon step \#2 completion, the system will return to the initial status of zero surplus and zero debt, users can burn their kUSD and reclaim equivalent value of collateral assets.

\[emergency-shutdown [Source](https://github.com/AcalaNetwork/Acala/tree/master/modules/emergency-shutdown)\]  


