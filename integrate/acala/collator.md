# Collator

## Overview

Collators maintain parachains by collecting parachain transactions from users and producing state transition proofs for Relay Chain validators. In other words, collators maintain parachains by aggregating parachain transactions into parachain block candidates and producing state transition proofs for validators based on those blocks.

While Acala’s network security and consensus (nPoS) are provided by Polkadot Relay chain's Validator set, the Collator set of Acala will keep the network alive by collecting parachain transactions for validators to verify them. **Unlike validators, collators has nothing to do with security of the network, by being a parachain, the network is by default trustless and decentralized, and a parachain only needs one honest collator to be censorship-resistant.** Read more on Collators [here](https://wiki.polkadot.network/docs/learn-collator).

Acala's Collator node maintains a full-node service for the Polkadot Realy Chain, and a full-node service for the Acala network. Each service has its own http/ws RPC endpoints, P2P ports etc. The base path of the Polkadot full-node service is located inside of the base path of the Acala full-node service.



## Collator Node

### Spec Requirement

Same as [the Polkadot validator node requirement](https://guide.polkadot.network/docs/maintain-guides-how-to-validate-polkadot/#requirements).

### Run Node

Refer to the [Full Node Guide](full-node.md).

### Collator Configuration

#### **Key Management**

Acala Collator needs Aura session key. RPC `author_rotateKeys` or `author_insertKey` can be used to update session key.

#### **Registration**

* Acala is currently using the `collectorSelection` pallet from Statemint to handle collator registration. During the authorized Collator set phase, the required candidacy bond will be an unattainably high value to prevent public registration.
* Authorized providers will need to submit the public key of the Aura session key for the collator.

#### CLI

* `--collator` for the parachain part

### **Example CLI**

```
--base-path=/acala/data
--chain=acala
--name=collator-1
--collator
--execution=wasm
--
--chain=polkadot
```
