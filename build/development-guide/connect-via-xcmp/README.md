# Composable Chains

Currently, cross-chain message passing and parachains are only available on Polkadot's testnet [Rococo](https://wiki.polkadot.network/docs/en/build-parachains-rococo). Acala's testnet Mandala is now launched on Rococo, and is testing cross-chain token transfers, and other functionalities. 

Please contact us to be \#ComposableWith Acala! 

## Composable With Acala

* Acala Mandala PC2 is live on Rococ [here](https://polkadot.js.org/apps/?rpc=wss://rococo-rpc.polkadot.io#/parachains)

#### Step 1 Support Acala Tokens

To receive tokens issued on Acala chain such as aUSD, ACA, renBTC, LDOT etc, you need to configure Acala as the reserve chain, and add token symbols. Follow the steps as follows:

1. Add token to CurrencyId. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/primitives/src/lib.rs#L83) of Laminar adding aUSD.
2. Add text symbol for the CurrenyId. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/primitives/src/lib.rs#L101) adding "AUSD".
3. Integrate `Tokens` module into your runtime. 

#### Step 2 Install xToken & Configure Reserve Chain

`orml-xtokens` is an implementation of [Polkadot Cross-Consensus Message Format \(XCM\)](https://github.com/paritytech/xcm-format) for token transfers. The source code for xtoken is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/sw/rococo-v1/xtokens) and xcm-support is [here](https://github.com/open-web3-stack/open-runtime-module-library/blob/sw/rococo-v1/xtokens/src/lib.rs)

1. Install xToken to your chain. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/runtime/dev/src/lib.rs#L861-L960) of Laminar adding xToken.
2. Configure Acala as Reserve Chain for specified tokens
   1. Add token and its reserve chain. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/runtime/dev/src/lib.rs#L916) of adding Acala as reserve chain for aUSD.
   2. Configure reserve assets. [Example here](https://github.com/laminar-protocol/laminar-chain/blob/a07ea4aa75bce5d30a24ce2e7a506dda5e22013f/runtime/dev/src/lib.rs#L916).

#### Step 3 Make your tokens available on Acala

Add your token symbols and make a PR to Acala. [Example of](https://github.com/AcalaNetwork/Acala/pull/730) Plasm's PR for adding PLM.

#### Step 4 Open HRMP Channel

Your chain shall already be connected to Rococo as parachain. While XCMP \(Cross-chain Message Passing\) is sitll being implemented - that is sending cross-chain messages directly to each other without passing through the Relay chain, a stop-gap protocol HRMP \(Horizontal Relay-routed Message Passing\) is in place. 

Two parachains will need to open HRMP channel on either side to send and receive cross-chain messages. [Instructions here to open HRMP Channel](open-hrmp-channel.md).

## \#ComposableWith

All chains on Polkadot/Kusama shall be _**composable with**_ each other, from exchanging value to exchanging and altering states. For example, chains can not only transfer values trustlessly, they can also call pallet/smart contract functions of each other e.g. 1 click minting PolkaBTC on Interlay chain, transferring PolkaBTC to Acala, and collateralizing it for aUSD all in one transaction. 

Acala is currently composable with the following \(potential\) parachains, please PR to this Repo to add yourself:

* Plasm
* Interlay
* Phala

Please contact us if you'd like to try it out and run some cross-chain testing together with us!

