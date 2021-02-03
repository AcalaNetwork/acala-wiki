# Acala Console

Acala Console is used to communicate with an Acala Node, where you can query the state of the blockchain e.g. balances of accounts, block information etc., and execute transactions to interact with various runtime modules of the chain e.g. transfer a token, do a token swap on the DeX etc. 

Open the Acala Console here: [https://console.acala.network/](https://console.acala.network/).

The Console is a generated front-end provided by Polkadot that can connect to various Substrate nodes. To connect the Console to your particular node, open the dropdown menu on the top left corner

![](https://i.imgur.com/8G8Rnbe.png)

Open the `Development` section, select `Local Node` to connect to your local Acala node.

![](https://i.imgur.com/TygeyXu.png)

Select `Custom` to connect to a deployed node, and paste the Websocket URL to the `custom endpoint` input box. You can find deployed nodes [here](../../../../learn/get-started/public-nodes.md).

Then click `Switch` on the top, and wait for the page to refresh and connect to the network. If your current endpoint already matches your selection, the `Switch` button will be disabled.

### Check Balance

Click the `Developer` tab on the top navigation bar, and select `Chain State` in the dropdown list.

![](https://i.imgur.com/BvFEcsZ.png)

To perform a state query and get your account's balance, do the following:
1. Click on the `selected state query`, and select `tokens`. 

2. Select the `accounts` storage.

3. Select your account \(in the example `Alice` \) from the `AccountId` dropdown.

4. Select `Token` from the `CurrencyId` dropdown, and `DOT` as `Token: TokenSymbol`

5. Press `+` button to initiate the call.

![](https://i.imgur.com/5hdanQC.png)

Your account's DOT balance will be shown below.

![](https://i.imgur.com/nOB7L3k.png)

