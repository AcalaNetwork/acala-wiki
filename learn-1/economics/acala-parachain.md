---
description: Acala Parachain Launch Offering
---

# Acala Parachain

## Ways to Deploy on Polkadot

Polkadot is designed both to easily integrate with other blockchains (via bridges like Interlay and RenVM), and to host a number of heterogenous (independent) but interconnected chains (called parachain, like Acala). Polkadot empowers an ecosystem of domain-specific chains optimized for their use cases. Substrate-based chains like Acala will enjoy full-tech-stack e.g. EVM and smart contract capability, as well as continuous innovation and technological upgrade.

There are several ways to deploy an application/protocol on Polkadot, each has their trade-offs.

1. **As parachain** - it has the highest degree of customizability and flexibility both technically and economically; it has more 'permanent' access (within the parachain lease period) to Polkadot's shared security and cross-chain communication facility. It has higher cost to setup, bootstrap and maintain.
2. **As parathread** - it's similar to parachain technically, but uses a pay-as-you-go model for security and communication access, hence less overhead upfront and cheaper to run.
3. **As DApp on existing parachain/parathread** - for teams don't want to build and maintain a blockchain, they can choose to deploy on an existing chain. For example, DApps/protocols can deploy on Acala using runtime modules, or as smart contracts (both Solidity Contract via EVM or Rust-based via Ink! are available). Read more [here](https://github.com/AcalaNetwork/Acala/wiki/U.-Build-with-Acala).

## Acala Parachain

Acala Network aims to launch as the first parachains on both Kusama and Polkadot networks. Below are the general key points for launching a parachain on Polkadot mainnet (Kusama would be similar)

* there will be **a limited number of parachains** supported
* hence a market mechanism will be used by Polkadot to allocate a parachain slot - using auctions
* Polkadot will release **one parachain slot at a time, and one auction per slot at a time**, but slots will continue to be released every few weeks (exact timeframe TBD)
* **the auction is NOT a sales event**; a sum of DOTs are locked as bond for the lease period of a parachain, and will be refunded at the end of the lease
* **the lease periods** are available in 6-month chunks and maximum 24-month, each lease can only be one of the 4 options (6-month, 12-month, 18-month, 24-month)
* **a crowdfund module** is provided by Polkadot to crowd source DOT holders' support More details [here](https://polkadot.network/polkadot-parachain-slots/)

## Acala Parachain Launch Offering

The mechanics of parachain launch and offering is still being researched, tested and deployed. The process is subject to change, but these are the principles we aim for (Kusama again would be the same)

1. It does **NOT raise capital**, all DOTs will be refunded at the end of the lease guaranteed by the trustless crowdfund module provided by Polkadot. It results in a net positive for DOT holders.
2. It is a **token distribution event**: majority of ACAs are reserved to reward DOT holders supported the crowdfund. You can think of it as **consensus/network security mining**, users lock their DOTs to help Acala gain access to Polkadot's PoS shared security, and ACAs are distributed to them as reward.
3. It is to **bootstrap Acala network & community**: uses will receive all rewarded ACAs upfront with vesting schedule. Vested balances cannot be transferred, but the full balance can be used for other activities such as governance and staking. This helps us to build a strong and right intentioned community from day one.

## The Launch Process

### The Overall Launch Process

There are 3 networks for Polkadot each with different purposes

1. **Polkadot testnet** - purely for testing functionalities, without any economic values e.g. Westend, Rococo (parachain testnet)
2. **Polkadot canary network i.e. Kusama network** - a valued network via its network token KSM, allow us to test with real economic incentives. Kusama is also a live independent network of its own right. With lower barrier to entry, some chains may only launch on Kusama without landing on Polkadot. Read more [here](https://medium.com/polkadot-network/kusama-polkadot-comparing-the-cousins-170e4fe6c280).
3. **Polkadot mainnet** - the mainnet.

Acala follows the same mentality (read Acala's trilogy network [here](https://github.com/AcalaNetwork/Acala/wiki/1.-Get-Started#acala-trilogy-networks)).

1. **Acala testnet** - currently live [here](https://github.com/AcalaNetwork/Acala/wiki/1.-Get-Started#mandala-test-network), all tokens there are test tokens with NO value.
2. **Acala canary network i.e. Karura** - a valued network via token KAR, a crowdfund and a parachain offering will be run for Karura, and it will launch on Kusama.
3. **Acala mainnet** - a valued network via token ACA, a crowdfund and a parachain offering will be run for Karura, and it will launch on Polkadot.

Acala canary network - Karura launch will happen first, basically as soon as Kusama is ready for parachain auction/launch. Generally speaking mainnet could be a month or two afterwards (but exact timeframe TBD), so when Polkadot is ready for parachain auction/launch, then Acala mainnet launch offering will begin.

### Launching Acala on Polkadot Mainnet

Drilling down into Acala parachain launch on Polkadot mainnet (Kusama would be similar)

1. **Prepare Funds** - once Polkadot's parachain runtime upgrade is approved and enters the enactment period, we'd know more precisely when the Acala crowdfund will begin, and will inform you to prepare DOTs e.g. unbond your DOTs to get ready for it.
2. **Crowdfund** - an Acala crowdfund will start accepting DOT deposits at a given time (time TBD) prior to a Polkadot parachain auction.
3. **Parachain Auction** - once an auction starts, the Acala crowdfund will start bidding with a goal to win the auction and secure a parachain slot.
4. **Parachain Launch** - if the Acala crowdfund wins the auction, then it will be launched on Polkadot mainnet as parachain, then starts the Acala genesis. Transfers and other functionalities like stablecoin will gradually be enabled, details of this launch process will be revealed later. ACAs will be distributed upon genesis.
5. **Refund**
   * if the Acala crowdfund does not win the auction, then DOTs will be refunded (trustlessly) to their holders via the crowdfund module.
   * if Acala wins the auction, DOTs will be refunded at the end of the lease.

## Participate in Acala Parachain Launch Offering

This is still work-in-progress, and we will update as soon as we have more details. Below are a few key points for participating in Acala Parachain Launch Offering on Polkadot mainnet (Kusama would be similar):

* **only transferrable DOTs can participate**: for staked DOTs, will need to unstake and wait till the end of unbonding period; for vested DOTs, will need to wait till vesting completes.
* **once DOTs deposited**, they will be locked until the end of the parachain lease if Acala wins the auction, otherwise be refunded at the end of an unsuccessful auction.
* **DOTs locking period**: is the same as the parachain lease period e.g. if Acala leases a parachain slot for 12-month, then DOTs will be locked for 12-month + the time in crowdfund before auction ends. The lease period (with options of 6-month, 12-month, 18-month or 24-month) has not been determined, we will get more community feedback as the process evolves. The longer the lease, the more stability and predictability for the project, as once the lease ends, the project will need to bid for a new slot otherwise it will lose security from Polkadot.

We will announce the crowdfund way ahead of time so people have sufficient time to unbond staked DOTs and prepare funds (for example for Acala crowdfund on Polkadot, we will announce \~1.5 months earlier).

**Subscribe to **[**Acala Newsletter**](https://acala.network/newsletter-sign-up.html)** to receive updates on Acala’s parachain launch.**

