# Composable Chains

Currently, cross-chain message passing and parachains are only available on Polkadot's testnet [Rococo](https://wiki.polkadot.network/docs/en/build-parachains-rococo). Acala's testnet \(Mandala\) is now launched on Rococo, and is testing cross-chain fungible token transfers, and other functionalities.

Please contact us in our [Discord tech chat](https://discord.gg/Xb3CxcjCVc) to be \#ComposableWith Acala!

## Composable With Acala

* Acala Mandala PC2 is live on Rococo [here](https://polkadot.js.org/apps/?rpc=wss://rococo-rpc.polkadot.io#/parachains)
* Refer to the xtokens documentation [here](https://github.com/open-web3-stack/open-runtime-module-library/wiki/xtokens)

### Background

[Polkadot Cross-Consensus Message Format \(XCM\)](https://github.com/paritytech/xcm-format) is a generic message format that doesn't specify use cases like fungible tokens. Therefore, we need to provide an implementation of the required use case e.g. cross-chain transfer, for parachains to be interoperable with the same context, namely, send/receive fungible assets between parachains, and between relay chain and parachains. We want to keep the same interface for Relay Chain assets \(like DOT or KSM\), and for native parachains assets \(like ACA for Acala\), and abstract from implementation details making it easy to integrate.

The [XCM Fungible Asset Implementation Guide](https://github.com/open-web3-stack/open-runtime-module-library/discussions/385) has laid out cross-chain fungible asset design considerations and discussions, as well as a reference implementation orml-xtokens that Acala and many others are currently adopted and testing.

`orml-xtokens` is a reference implementation of XCM for fungible tokens. The source code for xtokens is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) and xcm-support is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xcm-support).

Currently, the `xtokens` codebase is under development by Acala, please reach out to us in [Discord tech chat](https://discord.gg/Xb3CxcjCVc) if you need support.

## Integration Guide

Follow [this guide](https://hackmd.io/dhmCATb_QqygCPxkxaDcmA) by @bertstachios to setup a local parachain testnet environment.

### Step 0 Local Parachain Testnet

Follow [this guide](https://hackmd.io/dhmCATb_QqygCPxkxaDcmA) by @bertstachios to set up a local parachain testnet environment.

### Step 1 Support Acala Tokens

To receive tokens issued on Acala's chain such as aUSD, ACA, renBTC, LDOT etc, you need to include them in your currency type; and also, to implement currency id conversion.

Check example [here](https://github.com/AcalaNetwork/Acala/blob/master/runtime/acala/src/lib.rs#L1307) for currency id conversion in Acala runtime.

### Step 2 Make your token available in Acala

There is an onboarding procedure to introduce new tokens on Acala to avoid spam tokens. If it's for testing purpose, you could always skip this step, and `orml-unkown-tokens` will handle all the received foreign tokens.

### Step 3 Integrate `xtokens` module

`xtokens` module provides an interface for cross-chain assets transfer, using XCM messages.

Check out the implementation of [XCM for token transfers](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) and implementation of [XCM-support](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xcm-support).

You can check the [example of `xtokens` module integration](https://github.com/AcalaNetwork/Acala/blob/3c5da19e6031df91184106057fdcf73ba8784a29/runtime/mandala/src/lib.rs#L1517-L1658) for more details.

> Note: the reference implementation is by no means definitive, rather it is the starting point for the parachain community to experiment and iterate. Please provide feedback to [`xtokens`](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) or the [implementation guide](https://github.com/open-web3-stack/open-runtime-module-library/discussions/385).

### Step 4 Open HRMP Channel

Your chain shall already be connected to Rococo as a parachain. While XCMP \(Cross-chain Message Passing\) is still being implemented - that is sending cross-chain messages directly to each other without passing through the Relay chain, a stop-gap protocol HRMP \(Horizontal Relay-routed Message Passing\) is in place.

> HRMP has the same interface and functionality as XCMP but is much more demanding on resources since it stores all messages in the Relay Chain storage. When XCMP has been implemented, HRMP is planned to be deprecated and phased out in favor of it.

The two parachains will need to open HRMP channel on either side to send and receive cross-chain messages. [Instructions here to open HRMP Channel](open-hrmp-channel.md).

## \#ComposableWith

All chains on Polkadot/Kusama shall be _**composable with**_ each other, from exchanging value to exchanging and altering states. For example, chains can not only transfer values trustlessly, they can also call pallet/smart contract functions of each other e.g. minting PolkaBTC on Interlay chain, transferring PolkaBTC to Acala, and collateralizing it for aUSD all in one transaction.

Acala will be composable with the following \(potential\) parachains. If you have or are implementing `xtokens`, please PR to this Repo to add yourself:

* Plasm
* Interlay
* Phala
* Laminar
* Moonbeam
* Centrifuge
* HydraDX
* Darwinia
* Kilt
* Crust
* Snowfork
* Bit.Country
* Kabocha
* ...

Don't hesitate to contact us if you'd like to try it out, need support, and/or want to run some cross-chain testing together with us!

