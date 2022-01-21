# Overview

Acala takes a phased approach to employ various governance mechanisms that will allow it to progressively decentralize and ultimately be commanded by the majority network stakeholders. Acala's governance framework is based on Polkadot's technology that employs a Referenda chamber, a General Council, and a Technical Committee to govern the network.

Acala however has sub councils that manage specialized aspects of the network including the Financial Council and the Liquid Staking Council.&#x20;

Find out more on Acala Launch Phases and progressive decentralization roadmap [here](https://www.notion.so/acala/dcabf9ba7c6246c69b913d5972503227?v=4121894373fd43d98ffcac260803928d).&#x20;

## Referenda

Referenda is a simple, inclusive, stake-based voting scheme. Referenda can be started by public proposals or council proposals. There is a 8-day enactment delay associated with it. Emergency proposals (e.g. fix urgent network issues) can be "fast-tracked" to have a shorter enactment period.&#x20;

Acala experiments with Polkadot's voting mechanisms including Tallying, Voluntary Locking, Adaptive Quorum Biasing. Read more [here](https://wiki.polkadot.network/docs/learn-governance/#referenda).&#x20;

## Councils

### General Council

Acala will initially be governed by a Referenda chamber together with a General Council appointed by the Acala Foundation whose decisions regarding the network such as runtime upgrades, resolving network issues and improvements are made transparent on-chain. Meanwhile, any ACA holders can propose any changes to the network, protocols and the Acala Treasury will be collectively voted for or against via the Referenda chamber. The General Council then provides oversight with veto rights to stop proposals that may deem malicious, posing security risks or not in the best interest of the Acala network.

Once the network is sufficiently bootstrapped, stabilized, and security measures are in place, a Referenda will be started to move governance to the Elected Council phase where the candidacy of councilors is open, and councilors are elected by public voting.&#x20;

### Financial Council

Overseeing updates of stablecoin parameters, DeX parameters, Liquid Staking fees, and protocol tasks.

* Elected by the General Council via 2/3 approval rating.&#x20;

### Liquid Staking Council

Overseeing updates of Liquid Staking parameters e.g. validator selection

* Elected by LDOT holders.

### Oracle Collective

Electing Oracle operators. Membership of the [Oracle Gateway](../../../learn/basics/oracle/) requires approval from the General Council, which is essentially a Proof-of-Authority model that only authorized trusted operators can provide price feeds into the network. This model will evolve with the contemporary R\&D on the Oracle problem.

* Elected by the General Council via 2/3 approval rating.&#x20;

## Technical Committee

Fastracking emergency proposals that are critical to the network operation, delaying an enactment, and canceling uncontroversially dangerous proposals.&#x20;

* Elected by the General Council via 2/3 approval rating.&#x20;

## Emergency Actions

* Fasttrack a scheduled task to 12 hours+
  * Requires: 1/3+ Technical Committee consensus
* Fasttrack a scheduled task to <12 hours
  * Requires: 2/3+ Technical Committee consensus
* Delay a scheduled task for up to 48 hours
  * Requires: 1/3 Technical Committee consensus
* Cancel a scheduled task
  * Requires: from schedule Origin OR
  * 3/4+ General Council consensus
