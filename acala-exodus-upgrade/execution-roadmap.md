# Execution Roadmap

{% hint style="warning" %}
The following documentation outlines the intended features and processes that Acala Exodus Upgrade intends to implement in the future. This document will continue to be updated and reviewed as we progress with the Exodus Upgrade. Please note that certain sections may contain inaccuracies or outdated information.
{% endhint %}

## Execution Roadmap

<figure><img src="https://lh6.googleusercontent.com/9mtrU6kxF3L3547AoNJhwpL1pj4M-esxh71KUxPk-qghvPDbao5mKObkPzRf7-77a7tEWABxhgMweoBOaSTp8uyttEzxqZ0mbQ_iwu-g8wBo8wq-OE_YGhphcnepwaUmkBbyj8if3ZyqoAwTOFFJSas" alt=""><figcaption></figcaption></figure>

## Execution Status

* [x] Approval Vote ([here](https://twitter.com/AcalaNetwork/status/1669606148209778688?s=20))
* [x] aSEED I: aUSD Conversion ([here](https://twitter.com/AcalaNetwork/status/1681919546691842049?s=20), [here](https://twitter.com/AcalaNetwork/status/1679339060857556993?s=20))
* [ ] aSEED II: Vault Conversion
* [ ] aSEED III: Redemption
* [x] ACA I: Emission ([targeting Aug 15](https://twitter.com/AcalaNetwork/status/1678955311561076737?s=20))
* [ ] ACA II: Staking
* [ ] ACA III: Security, Dynamic Voting
* [ ] UHA I: Acala Multichain Asset Router & LSTs
* [ ] UHA II: Euphrates&#x20;
* [ ] UHA III Multichain

To kick off the Exodus Plan, an approval vote will be put forward for the general direction proposed. Each component of the Exodus Plan will be rolled out in phases.

### ACA Tokenomics I: Emission

Proposal and vote for new ACA emission and burn schedule. On-chain Treasury can be used to store the emission before actual distribution to pools and dApps etc. as some of which will require specific governance voting.

### ACA Tokenomics II: Staking

Deliver the ACA Farm pallet to provide ACA Staking and receive multiple reward tokens. ACA holders (including stakers) will vote for ACA emission distribution to liquidity programs. LST projects and dApps can deposit token contributions to the ACA Farm pallets.

### ACA Tokenomics III: Add Security, Dynamics Voting

Deliver pallet code to use locked assets in case of network shortfall. Proposal and vote for security parameters. In addition, there is more optimisation of II, delivering pallets that enable the following functionalities: voting for ACA emission distribution can be dynamically weighted based on vote participation, and LST projects and dApps token contributions to ACA Farm can be done via Smart Contract that is tied to emission governance, making the entire process more autonomous.

### aSEED I: aUSD Conversion

Prepare proposal and vote for parameter changes to aUSD Honzon protocol e.g. setting debt ceiling = 0 to cap aUSD issuance, pause oracle and liquidation etc. Prepare proposal and vote for aUSD conversion to aSEED such that all aUSD holders will then hold aSEED, aUSD pools will become aSEED pools, aUSD LPs will become aSEED LPs. aUSD and aUSD pools on other parachains can be coordinated for the conversion to continue support aSEED. Vault owners can still close vaults.

### aSEED II: Vault Conversion

Deliver the conversion pallet that can take % of the collateral in a vault, put it into the aSEED treasury and return the remaining collateral to the vault owner. This can be voted to execute at a specific later time so that vault owners would have sufficient time to prepare.

### aSEED III: Redemption

Prepare proposal and vote for redemption criteria e.g. after 12 months and aSEED underlying value >= $1. Deliver redemption pallet code for aSEED redemption for aSEED treasury assets.

### UAH I: Infrastructure and onboard/boost LSTs

Launch UAH infrastructure such as Acala Multichain Asset Router, launch liquidity programs for LST pools and partners

### UAH II: Euphrates dApp

Deliver the smart contract dApp on UAH, initially supporting boosted vaults for DOT LSTs. This will be one of the first examples where ACA emission gets distributed to boost a dApp, and LST project tokens are contributed to ACA Farm.

### UAH III: Multichain LSTs

Launch a pipeline of other L1 LSTs and LSTFi dApps. Incubate or deploy dApps that enable DOT holders to get exposure to multichain e.g. Ethereum staking yield and vice versa.

\
