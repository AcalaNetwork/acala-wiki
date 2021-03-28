# Deploy Contracts

The `Acala EVM Playground` is useful to test various functionalities of Acala EVM. It’s a fork from parity `canvas-ui`.

## **1. Setup**

To deploy your smart contract you can use our testnet or you can run your local dev node.

### Run your own dev node\*\*

To run your dev node, you can: 1. Build Acala project locally. You can follow the guide on how to do it here: [https://github.com/AcalaNetwork/Acala\#3-building](https://github.com/AcalaNetwork/Acala#3-building). Once you have built Acala you can start an EVM ready local dev node by running the following command:

```text
make run-eth;
```

1. Use the prebuilt Acala Docker image.

   You need to have Docker installed in your machine. You can follow the installation instructions here: [Install Docker](https://docs.docker.com/get-docker/). Once docker is running you need pull the last acala image and to run it:

   ```text
   docker pull acala/acala-node:latest
   docker run -p 9944:9944 acala/acala-node:latest --name "calling_home_from_a_docker_container" --rpc-external --ws-external --rpc-cors=all --dev
   ```

Once your node is running you should see something similar to this in your terminal window: ![](https://i.imgur.com/MQEURQr.png)

After your dev node is up and running you can set the Acala EVM playground to point to your node by clicking the dropdown in the bottom left corner and selecting `Local Node`. ![](https://i.imgur.com/pOfQb8z.png)

Running the local dev node will prepopulate the `Accounts` dropdowns with pre-funded developer accounts.

**Deploy to our test network**

To deploy to our test network you need to have the [polkadot{.js}](https://polkadot.js.org/extension/) wallet extension installed in your browser.

Once you have the extension installed, you can bind your accounts with an EVM address and get test network funds on the `Setup EVM Account` page on the Acala EVM playground. For more in-depth instructions, read the documentation [here](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/evm-account).

**Note:** For the remainder of this page we will assume you are using a local dev node. If you are deploying to the test network using the `polkadot{.js}` extension simply replace the accounts `Alice`, and `Bob` with the accounts you have set up in your extension.

## **2. Upload Contract ABI & bytecode**

Upload `BasicToken` ABI & bytecode file by navigating to [https://evm.acala.network/](https://evm.acala.network/).

Go to the `Upload` tab. ![](https://i.imgur.com/Ge3IwiM.png)

Fill in the contract name, then upload the contract file `BasicToken.json`.

![](https://i.imgur.com/kRM8Mfb.png)

It will then display a list of available methods in the contract. Then click `Upload`.

## **3. Deploy the Contract**

After uploading the ABI & bytecode file, the Playground will automatically navigate to the `Deploy` step. If not, just select `Deploy` in the left sidebar. `BasicToken` shall appear in the `ABI bundles`.

Click the `Deploy` button, and choose `Alice` \(or your account if using the browser extension\) as the `deployment account`.

![](https://i.imgur.com/FfoYEFU.png)

Set the initial supply \(e.g. `1000`\) and press `Deploy`.

![](https://i.imgur.com/wY0YG54.png)

After confirming the transaction you should be automatically navigated to the `Execute` section. Or you can navigate there manually by clicking "Execute" on the left sidebar. ![](https://i.imgur.com/wyrpMIv.png)

## **4. Interact with the Contract**

Navigate to the `Execute` tab. Find the deployed `BasicToken` contract and Click the `Execute` button on the bottom of the "BasicToken" box.

## **5. Query Balances**

To perform a query on an account's balance, do the following steps:

1. Select `Alice` from the `Call from Account` dropdown. This is the account used to send the transaction.
2. Pick `balanceOf` from the `Message to Send` dropdown.
3. Find Alice’s EVM Address under `Call from Account`, copy and paste it in the `account: address` input.

![](https://i.imgur.com/xH1j0ph.png)

**Note:** Solidity contracts have two types of methods: `views` and `executable` methods.

* `Views` are used to query information from the blockchain without writing data to it. `Views` transactions are free. The Playground uses the `Call` button to indicate this.
* `Executable` methods can write data onto the blockchain, and these transactions aren’t free. Click the `Execute` button to execute it.

Finally, click `Call` at the bottom, and `Call results` should show the BasicToken balance of Alice \(1000\).

![](https://i.imgur.com/GS7Znys.png)

## **6. Transfer**

Now let's try transferring BasicTokens to Bob’s Account.

1. Select `Alice` from the account dropdown.
2. Select `transfer` from the `Message to Send` dropdown.
3. Select Bob’s account \(don't forget to bind EVM address to BOB's account how we did for Alice\) from the `Call from Account` dropdown, then copy its `EVM address` and paste it in the `recipient address` input box. \(Remember to switch `Call from Account` back to `Alice`\)
4. Enter transfer amount in the `amount: unit256` argument box, note the token has a standard 18 decimals.
5. Click `Execute`.

![](https://i.imgur.com/l2utsuN.png)

A notification would pop-up to confirm the transaction is successful.

Now check the balances of Alice and Bob, and confirm that they have changed. Alice's account: ![](https://i.imgur.com/SCLwxRk.png) Bob's account: ![](https://i.imgur.com/pi3AKiN.png)

