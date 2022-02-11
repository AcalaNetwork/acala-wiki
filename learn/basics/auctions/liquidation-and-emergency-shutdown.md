# Liquidation & Emergency Shutdown

## Liquidation

If the collateral ratio of a loan is below the liquidation ratio, then it will be liquidated, meaning the collaterals of the loan will be sold for aUSD to payback outstanding debts. Depending on quantity, the collaterals will be sold on DeX automatically for acceptable price and slippage, and rest will be put on a collateral auction.

## As a Loan Owner

If your loan is at risk - that is the collateral ratio is close to the required liquidation ratio e.g. current collateral ratio is 150%, while the liquidation ratio is 135%, then a mere 10% price movement downwards will trigger liquidation. So as a loan owner, you will need to monitor the loan position closely, if your loan is at danger of liquidation, either **add more collateral to keep the loan afloat**, or **payback debts (by clicking the `Payback` button) to increase collateral ratio** to avoid liquidation.

**Risky Position**

* **RED**: Highly risky position, the current collateral ratio is very close to (on Mandala Testnet: within 10% of) the liquidation ratio.
* **YELLOW**: Medium risky position, the current collateral ratio is relatively close to (on Mandala Testnet: within 20% of) the liquidation ratio.
* **GREEN**: Safe position, the current collateral ratio is well above (on Mandala Testnet: above 20% of) the liquidation ratio.

To de-risk a position, click the `Deposit` button to add more collateral, or click the `Payback` button to payback aUSD debt.

**Highly Risky Position**&#x20;

![risky](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-risky.png)

**Medium Risky Position**

![](<../../../.gitbook/assets/image (2).png>)

## Participate in Liquidation Auctions

Acala uses [Off-Chain Worker](https://www.substrate.io/kb/learn-substrate/off-chain-workers) to automatically assess loan positions and liquidate loans. For liquidations with small amount of collaterals, it will be liquidated on the DeX automatically. The rest will be auctioned on collateral auctions, and users can participating in bidding the collaterals. More details on the process [here](https://github.com/AcalaNetwork/Acala/wiki#25-automatic-liquidations-of-risky-cdps). Read more on types of auctions [here](https://wiki.acala.network/learn/basics/auctions)

### On the DApp - Pulse

Log on the the Pulse App (link coming soon...), navigate to the `Liquidation` tab - `Collateral Auction` section, and you can see available auctions to participate in.&#x20;

![auction](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-auction.png)

### Monitor Events

These are relevant liquidation related events

* [**`LiquidateUnsafeCDP` event**](https://acala-testnet.subscan.io/event?module=cdpengine\&event=liquidateunsafecdp).
* [**`NewCollateralAuction` event**](https://acala-testnet.subscan.io/event?module=auctionmanager\&event=newcollateralauction)

You can also participate in these system debit & surplus auctions. ([https://github.com/AcalaNetwork/Acala/wiki/6.-Auctions](https://github.com/AcalaNetwork/Acala/wiki/6.-Auctions)).

* [**`NewDebitAuction` event**](https://acala-testnet.subscan.io/event?module=auctionmanager\&event=newdebitauction)
* [**`NewSurplusAuction` event**](https://acala-testnet.subscan.io/event?module=auctionmanager\&event=newsurplusauction)

### Guardian - Configurable Monitoring and Bot Framework

Monitor and participate in auction using the Guardian framework, see examples [here](https://github.com/open-web3-stack/guardian/tree/master/packages/example-guardian).

#### **Example: 1-Click Launch Collateral Auction Bot**

1. **Click the `Deploy on Heroku` button to launch Heroku**&#x20;
2. **Fill in the configuration**&#x20;

![heroku](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-guardian2.png)

![heroku](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-guardian1.png)

* **Bidder Address**: the account for the bot, make sure it has aUSD or other required trading assets
* **SURI**: this can be the derived path or mnemonic
* **Margin**:
* **Endpoint**: Latest WS endpoint [here](https://github.com/AcalaNetwork/Acala/wiki/1.-Get-Started#mandala-test-network)

#### **Example: CDP Guardian Bot for auto re-capitalization**

The CDP Guardian Bot will monitor your loan position, and automatically deposit more collateral to keep your loans afloat if the collateral ratio drops below a certain threshold. More [here](https://github.com/open-web3-stack/guardian/tree/master/packages/example-guardian#cdp-guardian-bot).

1. **Watch it bid 💰**&#x20;

![heroku](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-guiardian3.png)

### Acala Bot by Ryabina

Ryabina built a Telegram 🤖️ for Acala users to monitor their financial status - loan positions, liquidation events, balance changes, liquidity provision and returns. Join [here](https://t.me/Acala\_Ryabina\_bot).

Read the mini guide [here](https://medium.com/@ryabina/mini-guide-for-acala-network-telegram-bot-by-ryabina-92990e01120).

![ryabina](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-ryabina1.png)

**Add Alerts for aUSD Loans**&#x20;

![ryabina2](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-ryabina2.png)

**Add Alerts for Liquidation Auctions**&#x20;

![ryabina3](https://github.com/AcalaNetwork/Acala/wiki/image/liquidate-ryabina3.png)

## Emergency Shutdown

Emergency shutdown is the last resort to protect Acala's stablecoin system against serious security threats. The shutdown is triggered via Acala governance. It will enforce target prices of assets, and ensure aUSD and loan owners receive the value of assets they entitled to.

The system is capable of partial shutdown loans for single collateral or a global shutdown. Hereafter we will detail the global Emergency Shutdown procedure. When a global Emergency Shutdown is triggered, the following would happen:

1. **Shutdown triggered**: users are not allowed to update loan positions, the system will lock a target price for each asset, however users are free to withdraw any free collaterals i.e. collaterals that are yet to be used to borrow aUSD.
2. **Loan processing**: the system will stop/clear all auction, process outstanding loans and debts, this may take some time, but ultimately a basket of collateral assets will be available for aUSD holders to reclaim
3. **Reclaim debts**: upon step #2 completion, the system will return to the initial status of zero surplus and zero debt, users can burn their aUSD and reclaim equivalent value of collateral assets (which could be a mixture of renBTC, DOT etc). The mixture of collaterals returned will be determined by the system based on availability.

### Global Emergency Shutdown Guide

**Step 1** Shutdown starts... Users can withdraw their free collaterals i.e. collaterals that are not used/locked for aUSD loans. Other loan update operations are prohibited hereafter.&#x20;

![step1](https://github.com/AcalaNetwork/Acala/wiki/image/shutdown-1.jpg)

**Step 2** Liquidation... The system will clear all outstanding auction, loans and debts.&#x20;

![step2](https://github.com/AcalaNetwork/Acala/wiki/image/shutdown-2.jpg)

**Step 3** Reclaim aUSD collaterals... Upon Step 2 completion, aUSD holders and loan owners can burn aUSD to reclaim collateral assets.

![step3](https://github.com/AcalaNetwork/Acala/wiki/image/shutdown-3.jpg)

