# Deploy Contracts

The `Acala EVM Playground` is useful to test various functionalities of Acala EVM. It’s a fork from parity `canvas-ui`.

## **1. Setup**

To deploy your smart contract you can use our testnet or you can run your local dev node.

### Run your own development network

To run your own development network, you can follow the [instructions](https://evmdocs.acala.network/network/network-setup/local-development-network) on setting up your own development network in the official EVM+ documentation.

Once your development network is operational, you can connect your EVM wallet to it by using the following parameters:

<table data-header-hidden><thead><tr><th>Key</th><th>Value</th><th data-hidden></th></tr></thead><tbody><tr><td><strong>Name</strong></td><td>Mandala</td><td></td></tr><tr><td><strong>URL</strong></td><td><code>http://127.0.0.1:8545</code></td><td></td></tr><tr><td><strong>Chain ID</strong></td><td>595</td><td></td></tr><tr><td><strong>WS endpoint URL</strong></td><td><code>ws://127.0.0.1:9944</code></td><td></td></tr><tr><td><strong>Symbol</strong></td><td>ACA</td><td></td></tr></tbody></table>

You can reference the [instructions](connect-to-a-node/use-metamask-with-evm+.md) on how to connect MetaMask to the EVM+ and substitute the values from instructions with the values from above.

After your EVM wallet is connected to the Acala EVM+, you can continue to the [EVM playground](https://evm.acala.network/).

### **Deploy to our test network**

To deploy to our test network you need to have the [polkadot{.js}](https://polkadot.js.org/extension/) wallet extension installed in your browser.

Once you have the extension installed, you can [bind your accounts](evm-account.md#2.-bind-an-existing-ethereum-account) with an EVM address and get test network funds from the [Discord faucet](broken-reference).

{% hint style="info" %}
**Note:** For the remainder of this page we will assume you are using a local development network. If you are deploying to the test network.
{% endhint %}

## **2. Upload Contract ABI & bytecode**

To deploy a smart contract using [EVM playgrounds](https://evm.acala.network/), you need to compile your smart contract in your preferred development framework so that you have the ABI bundle available to upload.

<details>

<summary>How to create an <code>ExampleToken</code> ABI bundle</summary>

In case you want to use the same smart contract as it is used in this example, you can follow these short instructions on how to create it.

First clone the Acala Hardhat tutorials example:

```shell
git clone git@github.com:AcalaNetwork/hardhat-tutorials.git
```

Move into the examples repository and into the `token` example:

```shell
cd hardhat-tutorials/token
```

Within the example directory, install all of the dependencies and compile the smart contracts:

```shell
yarn && yarn build
```

This will compile the `Token` smart contract and create an ABI bundle to the directory `artifacts/contracts/Token.sol/` the bundle file is called `Token.json`.

</details>

Upload `ExampleToken` ABI & bytecode file by navigating to [https://evm.acala.network/](https://evm.acala.network/).

Go to the `Upload` tab.

![EVM playground => Upload](<../../../../.gitbook/assets/image (2).png>)

Assign the `Name` of your smart contract. You will be able to identify the smart contract in the `Deploy` tab with it, once it gets uploaded.

To upload the ABI bundle itself, you can either drag and drop it into the upload section, or click on the section and select the file.

![EVM playgrounds => Upload => Add file](<../../../../.gitbook/assets/image (44).png>)

Once you have selected the correct ABI bundle, the methods of the smart contract should be displayed. You can verify that the correct methods are listed and press `Upload` to upload the ABI bundle.

## **3. Deploy the Contract**

Smart contracts can be deployed under the [Deploy tab](https://evm.acala.network/#/deploy) of the EVM playgrounds.

The ABI bundles that you uploaded in the `Upload` tab can be seen here:

![EVM playgrounds => Deploy](<../../../../.gitbook/assets/image (10).png>)

The methods available for an ABI bundle can be seen by expanding the `ABI` menu. This can be helpful if you have multiple bundles uploaded and you want to be sure that you will be interacting with the right one.

![EVM playgrounds => Deploy => Expand ABI section](<../../../../.gitbook/assets/image (13).png>)

When you have verified that you are interacting with the ABI bundle that has the correct methods available, you can click `Deploy`, which should open a deployment interface:

![EVM playgrounds => Deploy => Deploy selected ABI bundle](<../../../../.gitbook/assets/image (1).png>)

The interface consists of the following components:

* Button to connect to your EVM wallet (this is why connecting MetaMask to the EVM+ is a prerequisite for this entry)
* Smart contract name, that can be changed, so you can deploy the same ABI bundle multiple times and easily differentiate between them
* ABI bundle identifications
* Fields to input the smart contract constructor parameters
* Value field to determine wether to send some of the native currency with the deploy transaction
* Fields to override the [`gas parameters`](https://evmdocs.acala.network/network/gas-parameters)
* `Deploy` button to deploy the smart contract once you are satisfied with the deployment parameters

### 1. Connect your EVM wallet

Pressing the <img src="../../../../.gitbook/assets/image (15).png" alt="" data-size="line"> button will prompt your EVM wallet to connect to the site. You can select the account that you want to use with the EVM playgrounds and connect it.

![](<../../../../.gitbook/assets/image (7).png>)![](<../../../../.gitbook/assets/image (14).png>)

The selected account should be displayed at the top of the page now:

![Displayed deployment account](<../../../../.gitbook/assets/image (19).png>)

### 2. Update the required deployment parameters

Depending on the requirements, you can modify the deployment parameters of your smart contract. It is required to fill out the constructor parameters, but modifying other values is optional.\


![Filled out deployment values](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FAr4HPdeSWiuUx1XzEALT%2Fuploads%2F9wxhZ96rDiIYMPvpYUKb%2Fimage.png?alt=media\&token=9afddcce-b216-42e2-bd55-24a7c0cd5cad)

Once the values are filled out and double checked, the smart contract is ready to be deployed.

{% hint style="warning" %}
The `validUntil` field value has to be higher than the current block number, or the deployment transaction will fail, due to the validator treating it as outdated. You can verify the current block number in a [block explorer](https://evmdocs.acala.network/network/gas-parameters).
{% endhint %}

### 3. Deploy the smart contract

Once the parameters of deployment are ready, you can deploy the smart contract by pressing the `Deploy` button. This should prompt your EVM wallet to confirm your deployment transaction:\


![Confirming the deployment transaction](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FAr4HPdeSWiuUx1XzEALT%2Fuploads%2FKRsWktz8uflxQILeGFkm%2Fimage.png?alt=media\&token=c5401b2a-5b30-4770-a1bf-018d1dd3fe2b)

Once the transaction is included in a block, the deployed smart contract can be found under `Execute` tab.

## **4. Interact with the Contract**

Navigate to the `Execute` tab. Find the deployed `ExampleToken` contract and Click the `Execute` button on the bottom of the "ExampleToken" box.

## **5. Query Balances**

To perform a query on an account's balance, do the following steps:

1. Make sure that the account you used to deploy the smart contract is connected to the `EVM playgrounds`.
2. Pick `balanceOf` from the `Message to Send` dropdown.
3. Copy and paste the address of the account that you used to deploy the smart contract to the `account: address` input.

![Filled out balance query](<../../../../.gitbook/assets/image (12).png>)

{% hint style="info" %}
**Note:** Solidity smart contracts have two types of methods: `views` and `executable` methods.
{% endhint %}

* `Views` are used to query information from the blockchain without writing data to it. `Views` transactions are free. The Playground uses the `Call` button to indicate this.
* `Executable` methods can write data onto the blockchain, and these transactions aren’t free. Click the `Execute` button to execute it.

Finally, click `Call` at the bottom, and `Call results` should show the ExampleToken balance of `123456789`.

![Completed balance query](<../../../../.gitbook/assets/image (23).png>)

## **6. Transfer**

Now let's try transferring ExampleTokens to another account.

1. Make sure that the deployer account is connected to the EVM playgrounds.
2. Select `transfer` from the `Message to Send` dropdown.
3. Fill out the `recipient address` input box with another EVM address to which to send the tokens.
4. Enter transfer amount in the `amount: unit256` argument box, note the token has a standard 18 decimals.
5. Click `Execute`.

![Filled out token transaction](<../../../../.gitbook/assets/image (43).png>)

A notification will pop-up to confirm that the transaction is successfully executed.

Now you can check the balances of both of the accounts, and confirm that they have changed. deployer's account:

![Deployer's balance](<../../../../.gitbook/assets/image (11).png>)

Other account:

![Other account's balance](<../../../../.gitbook/assets/image (46).png>)
