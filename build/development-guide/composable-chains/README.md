# Composable Chains

Currently, cross-chain message passing and parachains are only available on Polkadot's testnet [Rococo](https://wiki.polkadot.network/docs/en/build-parachains-rococo). Acala's testnet \(Mandala\) is now launched on Rococo, and is testing cross-chain fungible token transfers, and other functionalities.

Please contact us in our [Discord tech chat](https://discord.gg/Xb3CxcjCVc) to be \#ComposableWith Acala!

## Composable With Acala

* Acala Mandala PC2 is live on Rococo [here](https://polkadot.js.org/apps/?rpc=wss://rococo-rpc.polkadot.io#/parachains)

### Background

[Polkadot Cross-Consensus Message Format \(XCM\)](https://github.com/paritytech/xcm-format) is a generic message format that doesn't specify use cases like fungible tokens. Therefore, we need to provide an implementation of required use cases for parachains to be able to interoperate with the same context.

The [XCM Fungible Asset Implementation Guide](https://github.com/open-web3-stack/open-runtime-module-library/discussions/385) has laid out cross-chain fugible asset design considerations and discussions, as well as a reference implementation orml-xtoken that Acala and many others are curerntly adopted and tesitng. 

### Step 0 Local Parachain Testnet

Follow [this guide](https://hackmd.io/dhmCATb_QqygCPxkxaDcmA) by @bertstachios to setup a local parachain testnet environment.

### Step 1 Support Acala Tokens

To receive tokens issued on Acala's chain such as aUSD, ACA, renBTC, LDOT etc, you need to configure Acala as the reserve chain, and add token symbols. Follow the steps as follows:

1. Add token to CurrencyId. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/primitives/src/lib.rs#L83) of Laminar adding aUSD.
2. Add text symbol for the CurrenyId. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/primitives/src/lib.rs#L101) adding "AUSD".
3. Integrate `Tokens` module into your runtime. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/33e65efabff0ef1fdd359a8128a740378f884747/runtime/dev/src/lib.rs#L628-L670) to integrate xToken.

### Step 2 Implement XCM for Token Transfer

[Polkadot Cross-Consensus Message Format \(XCM\)](https://github.com/paritytech/xcm-format) is a generic message format that doesn't specify use cases like fungible tokens. Therefore, we need to provide an implementation of required use cases for parachains to be able to interoperate with the same context.

`orml-xtokens` is a reference implementation of XCM for token transfers. The source code for xtoken is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/sw/rococo-v1/xtokens) and xcm-support is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/sw/rococo-v1/xcm-support)

[Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/runtime/dev/src/lib.rs#L861-L960) of Laminar installing xToken to its chain.

### Step 3 Configure Reserve Chain

1. Configure Acala as Reserve Chain for specified tokens
   1. Add token and its reserve chain. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/runtime/dev/src/lib.rs#L916) of adding Acala as reserve chain for aUSD.
   2. Configure reserve assets. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/runtime/dev/src/lib.rs#L916).

### Step 4 Make your tokens available on Acala

There is an onboarding procedure to introduce new tokens on Acala to avoid spam tokens. Please contact us at hello@acala.network to discuss.

Once you are onboarded, add your token symbols and make a PR to Acala. [Example of](https://github.com/AcalaNetwork/Acala/pull/730) Plasm's PR for adding PLM.

### Step 5 Open HRMP Channel

Your chain shall already be connected to Rococo as a parachain. While XCMP \(Cross-chain Message Passing\) is still being implemented - that is sending cross-chain messages directly to each other without passing through the Relay chain, a stop-gap protocol HRMP \(Horizontal Relay-routed Message Passing\) is in place.

The two parachains will need to open HRMP channel on either side to send and receive cross-chain messages. [Instructions here to open HRMP Channel](open-hrmp-channel.md).

## \#ComposableWith

All chains on Polkadot/Kusama shall be _**composable with**_ each other, from exchanging value to exchanging and altering states. For example, chains can not only transfer values trustlessly, they can also call pallet/smart contract functions of each other e.g. minting PolkaBTC on Interlay chain, transferring PolkaBTC to Acala, and collateralizing it for aUSD all in one transaction.

Acala will be composable with the following \(potential\) parachains. Please [PR to this Repo](https://github.com/AcalaNetwork/acala-wiki/blob/master/build/development-guide/connect-via-xcmp.md) to add yourself:

* Plasm
* Interlay
* Phala
* Laminar
* Moonbeam
* Centrifuge
* HydraDX
* Darwinia
* ...

Please contact us if you'd like to try it out and run some cross-chain testing together with us!

