# Inter Kusama Transfer

🚨Karura is a "canary network" for Acala an early unaudited release of the code that is available first and holds real economic value. It is highly experimental and unstable.

No promise, and expect chaos.

## Why Transfer KSM to Karura

Because it’s cool - it might be the first-ever truly trustless cross-chain fungible token transfer in the world. So you are welcome to try it out and embrace the coolness and chaos of Kusama and Karura.

Besides, KSM is the native token of Kusama. It is used as the fee token, staking token to provide network security and governance token on the Kusama Relay chain. The launch of Karura parachain unlocks new use cases for KSM, where KSM can be used \(once these protocols are enabled\)

* As liquidity for KSM based trading pairs in the Karura Swap \(learn about becoming a liquidity provider & risks involved [here](swap/)\)
* As a collateral asset to borrow kUSD stablecoin, which can then be used for leveraged trades and other use cases
* To stake in the Karura Liquid Staking Pool, mint tokenized staked KSM as LKSM, which can then be used as collaterals or transferred, traded or else
  * LKSM is the programmable staking asset as the building block for many other protocols and applications
* ...

## Cross-chain Fungible Token Transfer 

Cross-chain transfer uses the Polkadot’s [Cross-chain Message Passing \(XCMP\)](https://wiki.polkadot.network/docs/learn-crosschain) technology, specifically the [Horizontal Relay-routed Message Passing \(HRMP\)](https://wiki.polkadot.network/docs/learn-crosschain#horizontal-relay-routed-message-passing-hrmp) as the basis for sending and receiving generic cross-chain messages. For sending and receiving fungible tokens, Karura has used the xtokens fungible token transfer implementation developed by Acala. 

You can find the source code: [xtokens](https://github.com/open-web3-stack/open-runtime-module-library/tree/3bf16d6efc8c35039a062748ff20fa6db6e8faa0/xtokens) and [xcm-support](https://github.com/open-web3-stack/open-runtime-module-library/tree/3bf16d6efc8c35039a062748ff20fa6db6e8faa0/xcm-support). 

You can also find the Cross-chain Message \(XCM\) Format developed by Parity [here](https://github.com/paritytech/xcm-format).

## Transfer KSM from Kusama to Karura

Use the [Karura App](https://apps.karura.network/portfolio), goto `Cross Chain` tab then select `Inter Kusama Transfer`. 

**Note:** The account you logged into the Karura App must have some KSM \(also be mindful of Existential Deposits\).

Select `Kusama` as the `From Chain`, and `Karura` as the `To Chain`. Your KSM balance \(on Kusama\) shall be displayed then. Then select the `To Account`, which can be the current account that you logged in to the Karura App. 

![](../../.gitbook/assets/screen-shot-2021-07-14-at-9.58.12-pm%20%282%29.png)

There are two parts to the transaction fees \(read more on fees [here](../get-started/transaction-fees.md)\)

* The `Origin Chain Transfer Fee` is charged by Kusama
* The `Destination Chain Transfer Fee` is charged by Karura, modeled closely with the Statemine chain

Then click \`Transfer\`, please be patient and it might take a few moments for the Kusama Relay chain to send and confirm your KSM to Karura 🚀

### Check Transaction

**On Kusama Side**

Go to the [Kusama Subscan Explorer](https://kusama.subscan.io/), search with your Kusama account, and you shall see the relevant `xcmpallet` transaction in the Extrinsics table.

![](../../.gitbook/assets/screen-shot-2021-07-14-at-10.08.45-pm.png)

**On Karura Side**

A Subquery will be implemented to make it easier to search and find your cross-chain transactions. Right now you can go to Karura Subscan to search Events where `module = parachainsystem` and `event = downwardmessagesprocessed`. OR [use this link](https://karura.subscan.io/event?address=&module=parachainsystem&event=downwardmessagesprocessed&startDate=&endDate=) to navigate there.

## Transfer KSM from Karura to Kusama

Use the [Karura App](https://apps.karura.network/portfolio), goto `Cross Chain` tab then select `Inter Kusama Transfer`. 

Select `Karura` as the `From Chain`, and `Kusama` as the `To Chain`. Your KSM balance \(on Karura\) shall be displayed then. Then select the `To Account`, which can be the current account that you logged in to the Karura App. Make sure your account is set as `Allow use on any chain` on the Polkadot{js} extension.

![](../../.gitbook/assets/screen-shot-2021-07-14-at-9.58.12-pm%20%281%29.png)

There are two parts to the transaction fee \(read more on fees [here](../get-started/transaction-fees.md)\)

* The ‘Origin Chain Transfer Fee\` is charged by Karura
* The \`Destination Chain Transfer Fee\` is charged by Kusama

Then click \`Transfer\`, please be patient and it might take a few moments for your KSM arrive back to Kusama 🚀

