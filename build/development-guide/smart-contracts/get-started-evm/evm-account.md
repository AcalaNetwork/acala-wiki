# Setup EVM Account

## **Single Wallet, Single Account Experience**

Users can use **one extension/wallet**, and **a single Substrate account** to interact with the Substrate runtime, contracts in EVM, and wasm contracts or a hybrid of these. If a user wants to use a particular Ethereum address, then simply link it with his/her Substrate address (basically proving the user owns both addresses), thereafter the user can just use the Substrate account with [Polkadot{js} extension](https://wiki.polkadot.network/docs/en/learn-account-generation) or alike to sign any Ethereum transactions seamlessly.

This allows users to use all functionalities within Acala and cross-chain capabilities without managing multiple accounts or wallets.

## Setup EVM Account

A user on Acala will always have a Substrate-based account that enables users to easily navigate multiple blockchains and sign any (EVM and Susbtrate) transactions with a single account. Read more on Acala Substrate Account [here](https://wiki.acala.network/learn/basics/acala-account). Follow the guide [here](https://wiki.acala.network/learn/get-started#create-a-polkadot-account) or [here](https://wiki.polkadot.network/docs/en/learn-account-generation) to generate a Substrate account.

To enable Single Account and use Acala EVM, you either

1. Bind an auto-generated Ethereum address OR
2. Bind an existing Ethereum account to the Substrate account

### **1. Bind an auto-generate EVM Account**

A user can generate an EVM address for each Substrate account. The user then can bind the EVM address to the Substrate account, so balances of native tokens e.g. DOT, renBTC, aUSD etc. on the Substrate account, are then available on the EVM address to use.

In the Acala EVM, if funds are sent to a Substrate account without an associated EVM address, an EVM address will be automatically generated and bound with the Substrate account.

Balances are automatically synchronized between the Substrate account and the associated EVM address. For example, a user teleports 10 renBTC to Acala, his/her balance will be shown in the Substrate account, the balance will also be shown and transferrable in the EVM address.

#### EVM Address Generation

The EVM Address is generated using the `blake2_256` hash function with a prefix `evm` and the associated Substrate account as input. Check out the source code [here](https://github.com/AcalaNetwork/Acala/blob/master/modules/evm-accounts/src/lib.rs#L185-L186).

```
blake2_256("evm:" ++ account_id)[0..20]
```

#### Claiming the default EVM address

Navigate to the [Polkadot.js web app](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Facala-mandala.api.onfinality.io%2Fpublic-ws#/extrinsics). The ability to claim the default EVM address can be found under `Extrinsics` in the `Developer` tab.

![Developer > Extrinsics](<../../../../.gitbook/assets/image (10).png>)

**Step 1: Select the Polkadot account**

If you haven't yet installed the Polkadot{js} extension and created an account, please do so by following the steps [here](https://wiki.polkadot.network/docs/en/learn-account-generation#polkadotjs-browser-plugin).

If the account is created and the extension is installed correctly, the account should be available in the `using the selected account` dropdown.

In case you have no funds in the Substrate account, please use the #acala-testnet-faucet channel in our [Discord](https://www.acala.gg/), to get some. You will need the funds to sent the transaction to bind the Substrate and EVM accounts.

**Step 2: Select the correct extrinsic**

To claim the default EVM address, select `evmAddress` from the `submit the following extrinsic` dropdown and select the `claimDefaultAccount()` option.

**Step 3: Claim the account**

Using the `Send transaction`, the Substrate wallet should prompt you to sign the transaction. Once the transaction is signed and added to the blockchain, your accounts should be bound.

![Sign and Submit transaction confirmation](<../../../../.gitbook/assets/image (22).png>)

**Step 4: Validate the account binding**

Once the accounts have been bound, you can validate the binding of the accounts, using the same Polkadot.js web app.

The chain state is validated using state queries, which can be found under `Developer` dropdown's `Chain state` option.

![Developer > Chain state](<../../../../.gitbook/assets/image (4).png>)

Select the `evmAccounts` from the `selected state query` dropdown. The `evmAddress(AccountId32)` option should return the EVM address bound to the Substrate account selected in the dropdown below.

Pressing the `+` button should query the chain for the associated EVM address and return it:

![Successful query for bound EVM address](<../../../../.gitbook/assets/image (1).png>)

### **2. Bind an Existing Ethereum Account**

In any case, if users want to use an existing Ethereum account in Acala EVM, this address will need to be claimed and bound to their Subatrate account.

_**One Substrate account can only be associated with one Ethereum address.**_ A Substrate address already linked to a generated EVM address can no longer link to an existing Ethereum address and vice versa.

Binding an existing Ethereum account requires users to prove they own the Ethereum account private key, by signing a message, include it in a `claim` transaction and send it to the Acala network.

#### Step 1: Get the genesis hash

* Select the **Metadata** from the **Settings** Section of the [Polkadot App](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fmandala-tc7-rpcnode.aca-dev.network%2Fws#/settings/metadata)
* Copy the **Genesis Hash** hex string

![Step 1: Getting the Genesis hash](<../../../../.gitbook/assets/image (34).png>)

#### Step 2: Get the Chain ID for your target address

* Select the **Developer** tab, then **Chain state** from the dropdown
* Select **Constant** and then **evm** from the **constant query** dropdown
* Choose **chainId** from the method/action dropdown
* Click the **+** button on the right

![Developer > Chain state > Constants > evm > chainId](<../../../../.gitbook/assets/image (31).png>)

#### Step 3: Create the signature of the claim on the [EVM+ Playground](https://evm.acala.network/#/Bind%20Account)

1. Select the right account in Metamask
2. Fill in the **Substrate address**, **Chain id** & **Genesis hash**
3. Click **Sign** & copy the **signature** to the next step

![Step 3: Create the signature of the claim](<../../../../.gitbook/assets/image (39).png>)

#### Step 4: Claim Account on the Developer Section of the [Polkadot App](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fmandala-tc7-rpcnode.aca-dev.network%2Fws#/extrinsics)

The **ethAddress** should be the same as your Metamask wallet address that you used above to generate the signature.

1. Select **evmAcounts** from the **extrinsic** dropdown menu
2. Select **claimAccount(ethAddress, ethSignature)** from the method/action dropdown
3. Fill in the **ethAddress** & **ethSignature**
4. Click **Submit Transaction**

![Step 4: Fill in eth address and eth signature](<../../../../.gitbook/assets/image (42).png>)

#### Step 5: Confirm the bindings

1. Select the **Developer** tab, then **Chain state** from the dropdown
2. Select **Storage** and then **evmAccounts** from the **state query** dropdown
3. Click the **+** button on the right
4. Double check that the **evmAccounts.evmAddresses** is indeed the right one.

![Developer > Chain state > Storage > evmAccounts > evmAddresses](<../../../../.gitbook/assets/image (35).png>)

#### Use Cases

Below are two potential use cases of binding an existing Ethereum address.

**Use Case 1**

For example, a DeFi protocol on Ethereum is now expanding its operation to Polkadot, by deploying their contracts on the Acala network. They will this new branch by airdropping tokens to their existing users if they also use the protocols on Acala.

The easiest way is to airdrop tokens to existing Ethereum addresses on Acala. Hence users would just bind their current Ethereum address to a Substrate address, use it for any EVM transactions, and receive airdrops.

**Use Case 2**

For DApps like [Linkdrop](https://linkdrop.io), users are required to sign messages using Ethereum private key. Using Linkdrop on Acala, would require users to claim their existing Ethereum address, and bind it to their Substrate account. Thereafter they can send transactions on behalf of the Ethereum account.
