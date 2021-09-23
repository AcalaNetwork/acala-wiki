# Collator

## Overview

Collators maintain parachains by collecting parachain transactions from users and producing state transition proofs for Relay Chain validators. In other words, collators maintain parachains by aggregating parachain transactions into parachain block candidates and producing state transition proofs for validators based on those blocks.

While Karura’s network security and consensus \(nPoS\) are provided by Kusama Relay chain's Validator set, the Collator set of Karura will keep the network alive by collecting parachain transactions for validators to verify them. **Unlike validators, collators has nothing to do with security of the network, by being a parachain, the network is by default trustless and decentralized, and a parachain only needs one honest collator to be censorship-resistant.** Read more on Collators [here](https://wiki.polkadot.network/docs/learn-collator%20).

Karura's Collator node maintains a full-node service for the Kusama Realy Chain, and a full-node service for the Karura network. Each service has its own http/ws RPC endpoints, P2P ports etc. The base path of the Kusama full-node service is located inside of the base path of the Karura full-node service.

## Collator Roll Out Plan

Karura takes a phased approach to roll out the Collator operation. Since Collators are non-security critical operation, a parachain only needs one honest Collator to be censorship-resistant, and more collators are not necessarily good e.g. it will slow down the network, the Collator roll-out plan is designed first-and-foremost to ensure network stability and operation.

**Current Phase: Private Collator Set**

**Phase 0: Private Collator Set**  
The Genesis of the Karura network was launched on 23rd June, 2021. Upon genesis, Karura's network security and the consensus is provided by Kusama Relay Chain's nominated Proof-of-Stake \(nPoS\) validators. Just like Statemine \(the common-good asset parachain on Kusama\), Karura's Collators initially will be run by the Acala Foundation, until the Collator software is stable and can be released to the wider community.

**Phase 1: Authorized Collator Set \(We are here\)**  
Through governance approval, Karura will then open the Collator set to an authorized set of collators. While there are no block rewards nor additional incentives for these authorized collators, they are paid a reasonable rate for their node service provisioning by the Acala Foundation.

These collators are known reputable node service providers who have a proven track-record of service levels and demonstrated a deep commitment to the network. It is expected that there will be much chaos and software upgrades during this phase still, and these collators are required to work closely with the core dev team to ensure network stability.

It is expected that the Karura network will maintain such an authorized Collator Set until the network is fully stabilized, the collator staking module and the reward scheme are fully implemented and audited.

These are the current collators for Karura added via Council Governance on-chain.

```text
Figment tbjKCiq9k2p6L4xLJ9H7mAHs8FBE76aG4KcDapUbUnifjnr
OnFinality sZDwoS5wHNrdXVe2BZLvgjw62Xri9Qo6bRa5che8EX5Yjx4
P2P - qCuATNWXzWaja6FpDkSpJnVa7MxSiHGaGa4qiFVsbx7cAQ3
staker.space op7G2ofug2Q2n1Bq3uUGzkNdSp68S93beMQkjjyDxnPREnk
Hypersphere sH4ZfKdzj58B3Bn22eFpgKXJBaGqcpCtd6qvXQRPVZWaPWD
BisonTrails sBwVWToF55CbuyBz3WEqDggniu8Gx18DkTQg4cagsCJsgVf
BlockDaemon qyf2KVDbBmF3MjRgmyFpQrNUnoxhp9AWCJrywysQcwXaiyb
Huobi Pool tN23BTgmWj7Z1buNSPyCLP9hDorWGxHAmh4ZYopjsRBdXLv
```

**Phase 2: Public Collator Set**  
Through governance approval, Karura will then enable the permissionless election of Collators and enable collator rewards. Due to the fact that Collators are non-security critical, a parachain only needs a small set of Collators to ensure liveness and censorship-resistance, the reward scheme will reflect this accordingly.

You can express interest in becoming a collator/liveness provider for the Karura Network \(and later Acala Network\) by [filling in this form](https://forms.gle/WQesfKVZmJeXov1e9). 

You can subscribe to the [`Node Operator - Announcement`](https://discord.gg/uWSZWsUcEn) channel on our Discord for updates and breaking changes.

## Collator Node

### Spec Requirement

Same as [the Kusama validator node requirement](https://guide.kusama.network/docs/mirror-maintain-guides-how-to-validate-kusama/#requirements).

### Run Node

Refer to the [Full Node Guide](full-node.md).

### Collator Configuration

#### **Key Management**

Karura Collator needs Aura session key. RPC `author_rotateKeys` or `author_insertKey` can be used to update session key.

#### **Registration**

* Karura is currently using the `collectorSelection` pallet from Statemint to handle collator registration. During the authorized Collator set phase, the required candidacy bond will be an unattainably high value to prevent public registration.
* Authorized providers will need to submit the public key of the Aura session key for the collator.

#### CLI

* `--collator` for the parachain part

### **Example CLI**

```text
--base-path=/acala/data
--chain=karura
--name=collator-1
--collator
--execution=wasm
--
--chain=kusama
```

