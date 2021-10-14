# Polkadot Explorer

Polkadot Explorer is used to communicate with an Acala Node, where you can query the state of the blockchain e.g. balances of accounts, block information etc., and execute transactions to interact with various runtime modules of the chain e.g. transfer a token, do a token swap on the DeX etc. 

Open the [Polkadot Explorer]((https://polkadot.js.org/apps/)).

The Console is a generated front-end provided by Polkadot that can connect to various Substrate nodes. To connect the Console to your particular node, open the dropdown menu on the top left corner

![](https://i.imgur.com/8G8Rnbe.png)

Open the `Development` section, select `Local Node` to connect to your local Acala node.

![](https://i.imgur.com/TygeyXu.png)

Select `Custom` to connect to a deployed node, and paste the Websocket URL to the `custom endpoint` input box. You can find deployed nodes here \[TODO\]. 

```text
wss://node-6757141250775003136.rz.onfinality.io/ws?apikey=086df60c-6a2d-414e-add2-cc0b74b6d00b
```

Then click `Switch` on the top, and wait for the page to refresh and connect to the network. If your current endpoint already matches your selection, the `Switch` button will be disabled.

### Check Balance

Click the `Developer` tab on the top navigation bar, and select `Chain State` in the dropdown list.

![](https://i.imgur.com/BvFEcsZ.png)

Then click on the `selected state query` , and select `token` . 

Select `Alice` from the `AccountId` dropdown.

Select `Token` from `CurrencyId` and `DOT` as `Token: TokenSymbol`

Press `+` button to initiate the call.

![](https://i.imgur.com/5hdanQC.png)

Alice's DOT balance will be shown below.

![](https://i.imgur.com/nOB7L3k.png)

