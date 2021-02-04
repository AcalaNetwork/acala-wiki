# Connect via Polkadot/Kusama

Currently, cross-chain message passing and parachains are only available on Polkadot's testnet [Rococo](https://wiki.polkadot.network/docs/en/build-parachains-rococo). Acala's testnet Mandala is now launched on Rococo, and is testing cross-chain token transfers, and other functionalities. 

### Token Transfer

We have built `orml-xtokens` to implement the [Polkadot Cross-Consensus Message Format \(XCM\)](https://github.com/paritytech/xcm-format) for token transfers. 

* The source code for xtoken is [here](https://github.com/open-web3-stack/open-runtime-module-library/tree/sw/rococo-v1/xtokens).
* Acala's Mandala testnet integration of this is [here](https://github.com/AcalaNetwork/Acala/blob/sw/rococo-v1/runtime/mandala/src/lib.rs)
* Acala Mandala PC2 is live on Rococ [here](https://polkadot.js.org/apps/?rpc=wss://rococo-rpc.polkadot.io#/parachains)

Please contact us if you'd like to try it out and run some cross-chain testing together with us!

