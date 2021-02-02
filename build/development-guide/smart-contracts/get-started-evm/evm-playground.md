# EVM Playground

We have created a web application - **Acala EVM Playground** to test various functionalities of Acala EVM. The source code is [here](https://github.com/AcalaNetwork/evm-playground) and it’s a fork from parity `canvas-ui`.

To launch the Playground, please navigate to [https://evm.acala.network](https://evm.acala.network).

By default, the Playground is connected to the Acala test network. It can also be connected it to a local node. If you've used the Playground before, the connection information may be cached.

### Set Up Node Connection

Click on the connection tab at the bottom left corner of the Playground.

![](https://i.imgur.com/9qnD9Gq.png)

Click on the `Node to connect to` dropdown to choose a node you want to connect to

* Select `Local Node` to connect to your local Acala node.
* Select `Acala` to connect to deployed Acala test network.
* Click `Use custom endpoint` to enter a custom Websocket URL

![](https://i.imgur.com/eHAdxLb.png)

### Check Balance

Let’s check Alice's DOT balance. On the left sidebar click `Execute`. 

A list of native token contracts would appear e.g. DOT, aUSD, ACA, renBTC, etc. These native tokens \(including cross-chain assets like renBTC\) are exposed as pre-compiled contracts that would otherwise not be available in an EVM. Their supply, balances on accounts and functions are all available in EVM.

Select `DOT` and press `Execute` under it.

![](https://i.imgur.com/gGqwRZM.png)

Pick Alice from the `Call from Account`. 

Pick `balanceOf` from `Message to Send`. 

Notice `EVM Address` under Alice's account, copy and paste it to the `owner: address` argument field. 

Click the `Call` button to execute.

![](https://i.imgur.com/8XQSarA.png)

The `Call results` at the bottom should show Alice’s DOT account balance.

![](https://i.imgur.com/2TNjbUM.png)

