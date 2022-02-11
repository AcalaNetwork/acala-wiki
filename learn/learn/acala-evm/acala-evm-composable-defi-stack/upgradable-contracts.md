# Upgradable Contracts

### **Upgradeable Smart Contracts.**

In Acala EVM, developers no longer need to write complex migration contracts to fix bugs or make improvements to existing applications. The contract maintainer can simply send a transaction with the new contract byte-code to seamlessly upgrade the contracts, without the need to `migrate` users nor liquidity.

There’s also a two-staged deployment process to reduce the risk of directly testing on mainnet with the public.

* **Deployed Private Contract**: Once a contract is deployed, it is by default private to the public and visible only to the `Contract Maintainer` \(who deployed the contract\) and opt-in developers. All necessary testing and final verification can be done before making it public. `Contract Maintainer` can also remove the contract at this stage if needed.
* **Deployed Public Contract**: `Contract Maintainer` can make the contract public aka open business to the public.

To incentivize users to be mindful of acquiring storage on a public ledger, and also in effect to reduce scams, we use `State Renting` mechanism. `Contract Maintainer` is required to put up a bond when deploying a contract, which will be refunded when the contract is removed from the chain.

Some barriers of entry would encourage more responsible behaviors, and we are constantly researching the space for better mechanisms to achieve this.

