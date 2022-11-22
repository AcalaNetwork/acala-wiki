# Wallet & Account

This document covers the basics of Acala, Karura, Polkadot and Kusama account addresses.

## Address Format

Acala and Karura use the Substrate-based chain address format SS58. Read more [here](https://wiki.polkadot.network/docs/en/learn-accounts).

* Acala addresses usually but not always start with the number 2.
* Karura addresses could start with a small letter like l, r, p, q, o...
* Polkadot addresses always start with the number 1.
* Kusama addresses always start with a capital letter like C, D, F, G, H, J...
* Generic Substrate addresses start with 5.

## Existential Deposit

Karura uses an [_existential deposit_ (ED)](https://wiki.polkadot.network/docs/learn-accounts#existential-deposit-and-reaping) to prevent dust accounts from bloating state. If an account drops below the ED, the state of this account will be removed from the blockchain to preserve scarce on-chain storage resources. The balance on this account will be removed and donated to the Treasury. You still retain access to the account, but it no longer has an on-chains state.

**Transfers:** when you transfer an amount from account A to account B

* if after the transfer, account A's balance is below ED, it will be removed. So make sure to leave enough balance on account A to keep it alive.
* if account B has no balance, and the transfer amount is below ED, account B would be as if never receive any amount, because its state would be removed from the chain. So make sure to send enough amount to keep a fresh account alive.

**Swap**: when you swap token A for token B, if token A balance then falls below ED requirement, then the transaction might fail. Anyone can build a front-end using acala.js SDK to facilitate this transaction and check ED for you, but you shall always be aware of it.

**Claim rewards**: when claiming LP tokens or other rewards, if the balance is below ED requirement after the claim, then the balance might be wiped.

ED applies to all supported token accounts, and each type of token account e.g. DOT account has its own ED requirement, meaning if DOT account balance is lower than ED, then your DOT balance may get wiped, while all other balances won't be affected.

Any transactions that change the balance of a particular token e.g. swap, then you shall be aware of its ED requirement. Here's the list of ED requirements for currently available tokens on Karura:

* ACA ED: 0.1 ACA
* aUSD ED: 0.1 aUSD
* DOT ED: 0.01 DOT
* LDOT ED: 0.05 LDOT

You can verify the existential deposit of ACA by checking the chain state for the constant `balances.existentialDeposit`

([ED Runtime Code](https://github.com/AcalaNetwork/Acala/blob/35078ea2b2d0e3a3937a075c54d94c77faea2f36/runtime/acala/src/lib.rs#L752-L754))

([ED SDK](https://github.com/AcalaNetwork/acala.js/blob/master/packages/sdk-wallet/src/utils/get-existential-deposit-config.ts))

## Account Generation&#x20;

You can generate Acala and Karura account in the following ways:

* Polkadot{.js} Browser Extension
* Polkawallet Mobile App
* Ledger Hardware Wallet



