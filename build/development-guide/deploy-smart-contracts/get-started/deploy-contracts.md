# Deploy Contracts

The `Acala EVM Playground` is useful to test various functionalities of Acala EVM. It’s a fork from parity `canvas-ui`.

### **1. Setup**

To deploy your contract on a node you have two options:

**Deploy to a local dev node**

To deploy to a local dev node you first need to follow the `Building` instructions here [https://github.com/AcalaNetwork/Acala#3-building](https://github.com/AcalaNetwork/Acala#3-building).

Once you have built Acala you can start an evm ready local dev node by running the following command:

```text
make run-eth
```

After your dev node is up and running you can set the Acala evm playground to point to your node by clicking dropdown in the bottom left corner and selecting `Local Node`.

![](https://i.imgur.com/4hndMwf.jpg)

Running the local dev node will preopulate the `Accounts` dropdowns with pre-funded developer accounts.

**Deploy to our test network**

To deploy to our test network you need have the [polkadot{.js}](https://polkadot.js.org/extension/) wallet extension installed in your browser.

Once you have the extension installed, you can bind your accounts with an evm address and get test network funds on the `Setup EVM Account` page on the Acala evm playground. For more in depth instructions, read the documentation [here](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/evm-account).

**Note:** For the remainder of this page we will assume you are using a local dev node. If you are deploying to the test network using the `polkadot{.js}` extension simply replace the accounts `Alice`, and `Bob` with the accounts you have set up in your extension.

### **2. Upload Contract ABI**

Deploy the `BasicToken` ABI file by navigating to [https://evm.acala.network/](https://evm.acala.network/).

Go to the `Upload` tab.

![](https://i.imgur.com/WEqKIwg.png)

Fill in the contract name, then upload the contract ABI `BasicToken.json`.

![](https://i.imgur.com/35cPG16.png)

It will then display a list of available methods in the contract. Then click `Upload`.

### **3. Deploy the Contract**

After uploading the ABI, the Playground will automatically navigate to the `Upload` step. If not, just select `Deploy` in the left sidebar. `BasicToken` shall appear in the `ABI bundles`.

Click the `Deploy` button, and choose `Alice` (or your account if using the browser extension) as the `deployment account`.

![](https://i.imgur.com/49maXFG.png)

Set the initial supply \(e.g. `10000000000000`\) and press `Deploy`.

![](https://i.imgur.com/ZQGxqny.png)

The deployed `BasicToken` contract should appear in the execute section.

![](https://i.imgur.com/BMp4SR3.png)

### **4. Interact with the Contract**

Navigate to the `Execute` tab. Find the deployed `BasicToken` contract.

Click the `Execute` button.

![](https://i.imgur.com/eOnKfQi.png)

### **5. Query Balances**

To perform a query on an account's balance, do the following steps:

1. Select `Alice` from the `Call from Account` dropdown. This is the account used to send the transaction.

2. Pick `balanceOf` from the `Message to Send` dropdown.

3. Find Alice’s EVM Address under `Call from Account` , copy and paste it in the `account: address` argument box.

![](https://i.imgur.com/ups2Ur6.png)

**Note:** Solidity contracts have two types of methods: `views` and `executable` methods.

- `Views` are used to query information from the blockchain without writing data to it. `Views` transactions are free. The Playground uses the `Call` button to indicate this.
- `Executable` methods can write data onto the blockchain, and these transactions aren’t free. Click the `Execute` button to execute it.

Finally, click the `Call` button and `Call results` should show the BasicToken balance of Alice.

![](https://i.imgur.com/URot8MK.png)

### **6. Transfer**

Now lets try transfering BasicTokens to Bob’s Account.

1. Select `Alice` from the account dropdown.

2. Select `transfer` from the `Message to Send` dropdown.

3. Select Bob’s account from the `Call from Account` dropdown, then copy its `EVM address` and paste it in the `recipient address` input box. (Remember to switch `Call from Account` back to `Alice`)

4. Enter transfer amount in the `amount: unit256` argument box, note the token has a standard 18 decimals.

5. Click `Execute`.

![](https://i.imgur.com/hPAGjsT.png)

A notification would pop-up to confirm the transaction is successful.

![](https://i.imgur.com/iXyEdFE.png)

Now check the balances of Alice and Bob, how have they changed?

![](https://i.imgur.com/mTLZGYU.png)

![](https://i.imgur.com/tTSS4gs.png)
