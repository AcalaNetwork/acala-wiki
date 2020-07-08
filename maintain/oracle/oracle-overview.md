---
description: Sources of asset pricing information.
---

# Oracle Overview

Oracles provide pricing information for assets on the Acala Network. Acala has designed its [oracle infrastructure](https://github.com/open-web3-stack/open-runtime-module-library/tree/master/oracle) to meet these demands through the implementation of the following features:

* **Multiple Providers**: The oracle module accepts data feeds from multiple nodes and data providers, and the aggregator combines the data to a single price.
* **Quality of Service**: Oracle operations are classified as high-priority transactions with a favorable fee schedule and DDoS attack protection, e.g. allow 1 call per block from each authorized provider to reduce delay, ensure reliability, and maintain cost-efficiency.
* **Progressive Decentralization**: Initially, oracle providers will be authorized and whitelisted to maximize security and predictability, while gradually shifting to be more permissionless and trustless over time.

Right now we whitelist a number of trusted operators to provide price feeds. We will take a phased approach to decentralizing our oracle system, starting with adding operator to `Operator Membership` by sudo account, then upgraded to general council approval. We are also following closely with the governance and technical progression in the space, and will gradually improve decentralization while maintaining highest security.

