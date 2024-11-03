# Composable Chains

Currently, cross-chain message passing and parachains are available on Polkadot/Kusama. Acala/Karura is now launched on Polkadot/Kusama, and is testing cross-chain fungible token transfers, and other functionalities.

## Composable With Acala

* Karura is live on Kusama [here](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama-rpc.polkadot.io#/parachains)
* Acala is live on Polkadot [here](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc.polkadot.io#/parachains)

### Background

[Polkadot Cross-Consensus Message Format (XCM)](https://github.com/paritytech/xcm-format) is a generic message format that doesn't specify use cases like fungible tokens. Therefore, we need to provide an implementation of the required use case e.g. cross-chain transfer, for parachains to be interoperable with the same context, namely, send/receive fungible assets between parachains, and between relay chain and parachains. We want to keep the same interface for Relay Chain assets (like DOT or KSM), and for native parachains assets (like ACA for Acala or KAR for Karura), and abstract from implementation details making it easy to integrate.

The [XCM Fungible Asset Implementation Guide](https://github.com/open-web3-stack/open-runtime-module-library/discussions/385) has laid out cross-chain fungible asset design considerations and discussions, as well as a reference implementation orml-xtokens that Acala and many others are currently adopted and testing.

Each parachain has its own fungible token, for parachains who want to do cross chain transfer of its asset and sibling parachain's token, [`orml-xtokens`](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) is a reference implementation of XCM for fungible tokens that can help easily doing cross-chain token transfer job.

> Note: the reference implementation is by no means definitive, rather it is the starting point for the parachain community to experiment and iterate. Please provide feedback to [`xtokens`](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) or the [implementation guide](https://github.com/open-web3-stack/open-runtime-module-library/discussions/385).

Parachains have it's own implementation to manage not only local asset, but also foreign asset. Currently, Acala use [asset-registry](https://github.com/AcalaNetwork/Acala/tree/master/modules/asset-registry). Thus, for parachains want to be added to Acala or Karura, please refer to the user guide [here](https://docs.acalaswap.app/developer-guides/create-a-new-token). Other parachain may have different asset management(i.e. use [pallet-assets](https://github.com/paritytech/substrate/tree/master/frame/assets) from substrate) or just manual maintain the assets, but they all use `orml-xtokens` doing cross-chain token transfer.

## Integration Guide

### Step 0 Local Parachain Testnet

Checkout README in [Acala main repo](https://github.com/AcalaNetwork/Acala), there're two tools to setup a local parachain testnet environment now:

* use parachain-launch which run with docker. Follow [this guide](https://hackmd.io/dhmCATb\_QqygCPxkxaDcmA) by @bertstachios.
* use polkadot-launch which run with binary build release file.

### Step 1 Integrate `xtokens` module to Support Acala/Karura Tokens

To receive tokens issued on Acala's chain(aUSD, ACA, renBTC, LDOT etc) or Karura's chain(kUSD, KAR, LKSM etc), you need to include them in your currency type; and also, to implement currency id conversion. Check [example here](https://github.com/AcalaNetwork/Acala/blob/2.3.1/runtime/acala/src/lib.rs#L1700-L1814) for currency id conversion in Acala runtime.&#x20;

> `CurrencyIdConvert` can convert `CurrencyId` to `MultiLocation` and also convert `MultiLocation` to `CurrencyId`. The former convert is used by orml-xtokens to send xcm to recipient chain, while the later one is used when you received xcm instruction that need to transfer to correct token.

### Step 2 Make your token available in Acala/Karura

There is an onboarding procedure to introduce new tokens on Acala/Karura to avoid spam tokens. Acala use [asset-registry](https://github.com/AcalaNetwork/Acala/tree/master/modules/asset-registry) to manage assets, please refer to the user guide [here](https://docs.acalaswap.app/developer-guides/create-a-new-token) to add new tokens on Acala/Karura.

### Step 3 Open HRMP Channel

> If you use parachain-launch or polkdot-launch, both tools support initiate hrmp channes when startup local testnet, so the cross-chain token transfer is ready to go. Of course you can manual init hrmp channels by the instructions below.

Your chain shall already be connected to Polkadot/Kusama as a parachain. While XCMP (Cross-chain Message Passing) is still being implemented - that is sending cross-chain messages directly to each other without passing through the Relay chain, a stop-gap protocol HRMP (Horizontal Relay-routed Message Passing) is in place.

> HRMP has the same interface and functionality as XCMP but is much more demanding on resources since it stores all messages in the Relay Chain storage. When XCMP has been implemented, HRMP is planned to be deprecated and phased out in favor of it.

The two parachains will need to open HRMP channel on either side to send and receive cross-chain messages. [Instructions here to open HRMP Channel](open-hrmp-channel.md).

## #ComposableWith

All chains on Polkadot/Kusama shall be _**composable with**_ each other, from exchanging value to exchanging and altering states. For example, chains can not only transfer values trustlessly, they can also call pallet/smart contract functions of each other e.g. minting PolkaBTC on Interlay chain, transferring PolkaBTC to Acala, and collateralizing it for aUSD all in one transaction.

Acala will be composable with the following (potential) parachains. If you have or are implementing `xtokens`, please PR to this Repo to add yourself:

* Bifrost
* Astar
* Interlay
* Phala
* Moonbeam
* Centrifuge
* HydraDX
* Darwinia
* Kilt
* Crust
* Snowfork
* Bit.Country
* ...

Don't hesitate to contact us if you'd like to try it out, need support, and/or want to run some cross-chain testing together with us!
