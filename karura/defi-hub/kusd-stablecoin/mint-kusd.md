# Mint kUSD

## Mint kUSD

Anyone can mint kUSD by depositing an accepted collateral e.g. KSM to create a vault/loan so called Collateralized Debt Position (CDP). 

You are required to put in these parameters when minting kUSD

* Select the type of collateral e.g. KSM
* Amount of the collateral to deposit 
* Amount of kUSD to mint

Note that the protocol does not directly record the amount of kUSD minted, but records a debit unit that accounts for kUSD minted plus accumulated stability fees etc.

### Key Parameters

The following are essential risks parameters and requirements of minting kUSD, note that each collateral type would have its own unique risk profiles:

* **Collateral ratio**: minted kUSD / dollar value of collateral deposited 
* **Required collateral ratio**: e.g. 200% means in order to mint 100 kUSD, you need to deposit at least $200 worth of collateral 
* **Current ratio**: your current collateral ratio
* **Stability Fee (payable in kUSD)**: this is a fee for minting kUSD, adjusting stability fee is an important instrument to manage supply and demand of kUSD hence the stability of dollar peg. This fee usually displayed as an annualized rate for indicative purpose only, in reality it is calculated each block and accumulated to the debit account. To close a kUSD vault, you will need to payback the kUSD minted as well as stability fees accrued. 
* **Liquidation ratio**: e.g. 185% means if the current ratio is below this, then the vault is deemed unsafe, the collateral will then be liquidated in exchange for kUSD to payback the vault. The price fluctuation of underlying collateral assets affects the risk profile of the minted kUSD, hence adjusting the liquidation ratio to a degree creates a stability shell of the stablecoin. 
* **Liquidation price**: if a collateral’s price drops to or under this price, then the vault is deemed unsafe. You can calculate this from the liquidation ratio, current ratio and the collateral amount. 
* **Liquidation penalty (payable in kUSD): **if liquidation (sell collateral for kUSD to pay back the vault) happens, you are required to pay this penalty. The liquidation penalty is a disincentive for users to leave a position in danger, hence provides additional safeguard and stability of the stablecoin.
* **Debt Ceiling**: maximum amount of kUSD minted by a particular collateral asset
* **Minimum kUSD in Vault:** e.g. 100 kUSD means you need to have at least 100 kUSD minted to maintain the vault, otherwise you will have to pay back the full outstanding minted kUSD amount.
* **Oracle prices**: the protocol relies on real-time market price for the collaterals to assess risks and trigger liquidations if thresholds are met. These market prices come from Acala Oracle via the Open Oracle Gateway

Read more on how these risk parameters are set [here](stability-and-liquidation/adjust-risk-parameters.md).

### Extrinsics

This is the Extrinsics (transaction) used to mint kUSD.

```
honzon.adjustLoan(currency_id, collateral_adjustment, debit_adjustment)
```

## Update kUSD Vault

Once you have minted kUSD and created the vault, you can update it in the follow ways:

* **mint more kUSD**, if there's enough collateral in the vault 
* **repay kUSD minted (owed)**, this will increase current collateral ratio and reduce stability fees needed to be paid
* **withdraw collateral**, if there's collateral not used for minting kUSD
* **deposit collateral**, to increase collateral ratio (be more safe in case of huge price fluctuation of underlying asset), and to mint more kUSD if needed

### Extrinsics

This is the Extrinsics (transaction) used to update kUSD vault. The same Extrinsics is also used to mint kUSD (open a vault). 

```
honzon.adjustLoan(currency_id, collateral_adjustment, debit_adjustment)
```

## Close kUSD Vault

There're two ways to close a kUSD vault

1. repay the exact amount of kUSD minted to 'close' the vault OR
2. use available collateral to pay it back, this is useful when you don't have enough kUSD at hand, and the system will automatically swap your collateral for kUSD to pay down the vault in a single transaction

### Extrinsics

This is the Extrinsics (transaction) that uses available collateral to pay it back

```
honzon.closeLoanHasDebitByDex(currency_id, maybe_path)
```

The Extrinsics to repay exact amount of kUSD is the same as mint & update kUSD vault. 

\[honzon [Source](https://github.com/AcalaNetwork/Acala/tree/master/modules/honzon)]\
