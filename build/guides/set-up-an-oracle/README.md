# Set up an Oracle

## Setting up a Provider

An oracle Network Provider can implement their own oracle pallets (based on [the default oracle pallet](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/oracle)) to meet their specific requirements such as validating cross-chain data feeds. We can also easily integrate with existing signed oracle APIs such as [Coinbase price oracle](https://blog.coinbase.com/introducing-the-coinbase-price-oracle-6d1ee22c7068) by simply adding a Coinbase Provider pallet and validate their signature.

Then add the oracle provider to the runtime (see [Acala Oracle](https://github.com/AcalaNetwork/Acala/blob/master/runtime/mandala/src/lib.rs#L447)) which would be enacted through runtime upgrade.&#x20;

### Setting up Operator Nodes

A Provider can add multiple operator nodes to its oracle network, an example operator node [here](https://github.com/laminar-protocol/oracle-server).

## Join the Open Oracle Gateway

The Oracle Gateway is deployed and running on Acala’s Mandala Test Network. In addition to the Acala team and Laminar team, the Band Protocol team has also contributed to the development of the Gateway. We hope the Gateway becomes a common good infrastructure for the Acala, Polkadot, Kusama and the cross-chain DeFi ecosystem in general. Therefore we welcome oracle service providers to check out the Gateway source code, talk to us about integration, contribute to the codebase and provide your services to an ever-growing ecosystem.

**Pre Acala/Karura genesis**

* please contact us directly (hello at acala dot network) or ask on [Discord](https://www.acala.gg/) to express your interest
* submit a Pull Request
* we'll review and approve PR
* then merge the code and deploy on the testnet

**Post Acala/Karura genesis**

* please contact Acala/Karura council to raise interest & seek approval
* upon approval, submit a Pull Request&#x20;
* code review and merge
* runtime upgrade
