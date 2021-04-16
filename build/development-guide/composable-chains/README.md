# Composable Chains

Currently, cross-chain message passing and parachains are only available on Polkadot's testnet [Rococo](https://wiki.polkadot.network/docs/en/build-parachains-rococo). Acala's testnet \(Mandala\) is now launched on Rococo, and is testing cross-chain fungible token transfers, and other functionalities.

Please contact us in our [Discord tech chat](https://discord.gg/Xb3CxcjCVc) to be \#ComposableWith Acala!

## Composable With Acala

* Acala Mandala PC2 is live on Rococo [here](https://polkadot.js.org/apps/?rpc=wss://rococo-rpc.polkadot.io#/parachains)
* Refer to the xtoken documentation [here](https://github.com/open-web3-stack/open-runtime-module-library/wiki/xtokens) 

### Background

[Polkadot Cross-Consensus Message Format \(XCM\)](https://github.com/paritytech/xcm-format) is a generic message format that is very flexible but loosely defined. Therefore, we need to provide an implementation of the required use case e.g. cross-chain transfer, for parachains to be interoperable with the same context, namely, send/receive fungible assets between parachains, and between relay chain and parachains. We want to keep the same interface for Relay Chain assets (like DOT or KSM), and for native parachains assets (like ACA for Acala), and abstract from implementation details making it easy to integrate.

The [XCM Fungible Asset Implementation Guide](https://github.com/open-web3-stack/open-runtime-module-library/discussions/385) has laid out cross-chain fungible asset design considerations and discussions, as well as a reference implementation orml-xtoken that Acala and many others are currently adopted and testing.

`orml-xtokens` is a reference implementation of XCM for fungible tokens. The source code for xtoken is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) and xcm-support is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xcm-support).

Currently, the `xtoken` codebase is under development by Acala, please reach out to us in [Discord tech chat](https://discord.gg/Xb3CxcjCVc) if you need support.

## Integration Guide

This guide shows how to set up cross-chain transfers between two parachains (**parachain A** and **parachain B**) for sending through their native tokens (**A-Token** and **B-Token** respectively) and **Relay Chain** assets (e.g. DOT in Polkadot).

Transfer messages are wrapped in XCM format and delivered using Horizontal Relay-routed Message Passing (HRMP) channels.

> While [XCMP](https://wiki.polkadot.network/docs/en/learn-crosschain) is still being implemented, a stop-gap protocol (see definition below) known as [HRMP](https://wiki.polkadot.network/docs/en/learn-crosschain#horizontal-relay-routed-message-passing-hrmp) exists in its place. HRMP has the same interface and functionality as XCMP but is much more demanding on resources since it stores all messages in the Relay Chain storage. When XCMP has been implemented, HRMP is planned to be deprecated and phased out in favor of it.

### Step 0 Local Parachain Testnet

Follow [this guide](https://hackmd.io/dhmCATb_QqygCPxkxaDcmA) by @bertstachios to set up a local parachain testnet environment.

### Step 1 Parachain A should include B-Token (native parachain B token).

To be able to accept native **B-Token**, **parachain A** needs to include it to the list of accepted currencies; and, also, to implement Currency ID Conversion. 

Currency ID Conversion should be implemented in runtime. Check example for Acala [here](https://github.com/AcalaNetwork/Acala/blob/master/runtime/acala/src/lib.rs#L1307).

To avoid spam tokens, parachain might have an onboarding procedure to introduce new tokens. Here is an [example of](https://github.com/AcalaNetwork/Acala/pull/730) Plasm's PR to Acala for adding PLM.

### Step 2 Parachain B should include A-Token (native parachain A token).

If we want to be able to send **A-Token** to **parachain B**, **parachain B** needs to repeat instructions from **Step 1**:
1. Add **A-Token** currency ID.
2. Implement Currency ID Conversion for **A-Token**.

### Step 3 Integrate `xtokens` module

**Xtoken** module provides an interface for transferring native tokens between parachains, using XCM message format.

Check out the implementation of [XCM for token transfers](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) and implementation of [XCM-support](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xcm-support).

You can check the [example of **xtokens** module integration](https://github.com/AcalaNetwork/Acala/blob/3c5da19e6031df91184106057fdcf73ba8784a29/runtime/mandala/src/lib.rs#L1517-L1658) for more details.

> Note: the reference implementation is by no means definitive, rather it is the starting point for the parachain community to experiment and iterate. Please provide feedback to [`xtokens`](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/xtokens) or the [implementation guide](https://github.com/open-web3-stack/open-runtime-module-library/discussions/385).

### Step 4 Open Horizontal Relay-routed Message Passing (HRMP) Channel

After setting up **Parachain A** and **B** for recognizing native tokens of each other, to activate cross-chain transfer we need to enable HRMP on both parachains. HRMP consists of unidirectional channels. Thus, for each  Parachain, we need to open two channels: one for sending messages; and another for receiving.

Please, check out [Instructions to open/configure HRMP Channel](https://wiki.acala.network/build/development-guide/composable-chains/open-hrmp-channel)


## \#ComposableWith

All chains on Polkadot/Kusama shall be _**composable with**_ each other, from exchanging value to exchanging and altering states. 

Below are (potential) parachains that have implemented or are implementing **xtokens**. If you have or are implementing **xtokens**, please PR to this Repo to add yourself:

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
* ...

Don't hesitate to contact us if you'd like to try it out, need support, and/or want to run some cross-chain testing together with us!

