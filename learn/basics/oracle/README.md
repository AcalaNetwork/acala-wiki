# Open Oracle Gateway

## Overview

The challenge of providing reliable, accurate and decentralized oracles cannot be solved by Acala alone. When taking into account that Acala is the DeFi hub and platform powering more cross-chain DeFi DApps on Polkadot, Kusama and beyond, creating a more open, inclusive, and decentralized oracle infrastructure with other leading projects in the industry becomes critical. The Open Oracle Gateway (OOG) is a significant step towards that vision.&#x20;

The Gateway allows multiple oracles to be deployed on the Acala network, leveraging Acala's DeFi optimized oracle infrastructure, and serving any DApps on Acala, Polkadot, Kusama and beyond.  Specifically, the Gateway offers:

1. **Multiple Oracle Networks**: multiple parties in addition to Acala Oracle can operate their own oracle services. Providers can set up their own oracle network with node operators, or integrate with an oracle API.&#x20;
2. **Price Feeds by Choice**: Dapps can choose to use an aggregated feed from all providers or a single provider, or they can obtain raw data from individual node operators and aggregate them themselves.&#x20;
3. **Quality of Service**: All price feeds posted through the Gateway will be provided with Quality of Service - these transactions are _operational transactions_ (system critical transactions) that are prioritized and guaranteed to be included in a block.
4. **FREE of Fees**: All valid feeds will be refunded with the transaction fees incurred, essentially making oracle feeds FREE for providers while preventing spam and ensuring integrity.&#x20;
5. **Progressive Decentralization**: Acala network will progressively decentralize, start with PoA (appointed Council governance), then evolve into elected Council governance and eventually democracy. The Gateway then initially will require sitting Council approval for accepting new oracle providers and their node operators.&#x20;

## Setting up a Provider

An oracle Network Provider can implement their own oracle pallets (based on [the default oracle pallet](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/oracle)) to meet their specific requirements such as validating cross-chain data feeds. We can also easily integrate with existing signed oracle APIs such as [Coinbase price oracle](https://blog.coinbase.com/introducing-the-coinbase-price-oracle-6d1ee22c7068) by simply adding a Coinbase Provider pallet and validate their signature.

Then add the oracle provider to the runtime (see [Acala Oracle](https://github.com/AcalaNetwork/Acala/blob/master/runtime/mandala/src/lib.rs#L447)) which would be enacted through runtime upgrade.&#x20;

### Setting up Operator Nodes

A Provider can add multiple operator nodes to its oracle network, an example operator node [here](https://github.com/laminar-protocol/oracle-server).

## Join the Open Oracle Gateway

The Oracle Gateway is deployed and running on Acala’s Mandala Test Network. In addition to the Acala team and Laminar team, the Band Protocol team has also contributed to the development of the Gateway. We hope the Gateway becomes a common good infrastructure for the Acala, Polkadot, Kusama and the cross-chain DeFi ecosystem in general. Therefore we welcome oracle service providers to check out the Gateway source code, talk to us about integration, contribute to the codebase and provide your services to an ever-growing ecosystem.

**Pre Acala/Karura genesis**

* please contact us directly (hello at acala dot network) or ask on [Discord](https://discord.gg/vdbFVCH) to express your interest
* submit a Pull Request
* we'll review and approve PR
* then merge the code and deploy on the testnet

**Post Acala/Karura genesis**

* please contact Acala/Karura council to raise interest & seek approval
* upon approval, submit a Pull Request&#x20;
* code review and merge
* runtime upgrade
