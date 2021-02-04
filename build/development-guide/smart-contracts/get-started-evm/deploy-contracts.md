# Deploy Contracts

The `Acala EVM Playground` is useful to test various functionalities of Acala EVM. The source code is [here](https://github.com/AcalaNetwork/evm-playground) and it’s a fork from parity `canvas-ui`.

### **1. Upload Contract ABI**

Deploy the `BasicToken` ABI file by navigating to [https://evm.acala.network/](https://evm.acala.network/).

Go to the `Upload` tab.

![](https://i.imgur.com/WEqKIwg.png)

Fill in the contract name, then upload the contract ABI `BasicToken.json`.

![](https://i.imgur.com/35cPG16.png)

Then it would display a list of available methods in the contract. Then click `Upload`.

### **2. Deploy the Contract**

After uploading the ABI, the Playground would automatically navigate to the `Upload` step. If not, just select `Deploy` in the left sidebar. `BasicToken` shall appear in the `ABI bundles`.

Click the `Deploy` button, and choose `Alice` as the `deployment account`.

Note: the test network has been pre-loaded with a few developer accounts, and Alice is one of the pre-funded accounts.

![](https://i.imgur.com/49maXFG.png)

Set the initial supply \(e.g. `10000000000000`\) and press `Deploy`.

![](https://i.imgur.com/ZQGxqny.png)

The deployed `BasicToken` contract should appear in the execute section.

![](https://i.imgur.com/BMp4SR3.png)

### **3. Interact with the Contract**

Navigate to the `Execute` tab. Find the deployed `BasicToken` contract.

Click the `Execute` button.

![](https://i.imgur.com/eOnKfQi.png)

### **4. Query Balances**

Pick `balanceOf` from the `Message to Send` dropdown.

Select `Alice` from the `Call from Account` dropdown. This is the account used to send the transaction.

Find Alice’s EVM Address under `Call from Account` , copy and paste it in the `account: address` argument box.

![](https://i.imgur.com/ups2Ur6.png)

Press `Call` and check out the returned results below.

**Note:** Solidity contracts have two types of methods: `views` and `executable` methods.

- `Views` are used to query information from the blockchain without writing data to it. `Views` transactions are free. The Playground uses the `Call` button to indicate this.
- `Executable` methods can write data onto the blockchain, and these transactions aren’t free. Click the `Execute` button to execute it.

![](https://i.imgur.com/URot8MK.png)

`Call results` should show the BasicToken balance of Alice.

### **5. Transfer**

Now try transfer BasicTokens to Bob’s Account.

Select Bob’s account from the `Call from Account` dropdown, then copy its `EVM address`.

Switch account back to Alice, select `transfer` from the `Message to Send` dropdown.

Paste Bob’s EVM address in the `recipient address` input box.

Then enter transfer amount in the `amount: unit256` argument box, note the token has a standard 18 decimals.

Click `Execute`.

![](https://i.imgur.com/hPAGjsT.png)

A notification would pop-up to confirm the transaction is successful.

![](https://i.imgur.com/iXyEdFE.png)

Now check the balances of Alice and Bob, how have they changed?

![](https://i.imgur.com/mTLZGYU.png)

![](https://i.imgur.com/tTSS4gs.png)
