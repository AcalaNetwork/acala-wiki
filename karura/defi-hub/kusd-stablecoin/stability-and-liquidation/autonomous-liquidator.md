# Autonomous Liquidator

The `cdp-engine` operates an autonomous liquidator in the form of [Off-Chain Worker](https://substrate.dev/docs/en/knowledgebase/learn-substrate/off-chain-features) that automatically checks each vault’s safety every block and triggers on-chain liquidation process for unsafe vaults. 

The autonomous Liquidator Off-Chain Worker is decentralized, more secure and efficient mechanism to safeguard the protocol compared to the traditional method of relying on external parties to liquidate \(e.g. Keeper servers\), because its code is an integral part of the blockchain runtime and guaranteed by the network, while does not take up on-chain resources for execution, hence **highly secure, scalable and efficient**. 

A vault is safe, if

$$
Current~Ratio > Liquidation~Ratio
$$

The `current ratio` is the debt to collateral ratio based on the current market value of the collateral asset, given by 

$$
Current~Rratio = minted~kUSD / Collateral~Market~ Price~($)
$$

Once the autonomous Liquidator identifies an unsafe vault, it will trigger the liquidation process to sell off the collateral for kUSD to repay the vault. The liquidation process uses a hybrid mechanism of liquidating on decentralized exchange and collateral auctions.

\[[Source](https://github.com/AcalaNetwork/Acala/blob/master/modules/cdp-engine/src/lib.rs#L488)\]

