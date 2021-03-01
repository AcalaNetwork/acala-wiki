# No Dust Account

### **Avoid Dust Accounts**

Dust accounts are accounts with very little funds, generally less than the amount needed to conduct a transaction. Too many dust accounts add unnecessary data to the blockchain, which would make it difficult for full nodes to sync with the network \(since every full node has a complete copy of the blockchain\).

On the Acala network, an address is only active when it holds a minimum amount \(exact number TBD\). This minimum amount is called “Existential Deposit” \(ED\), similar to [ED on Polkadot](https://support.polkadot.network/support/solutions/articles/65000168651-what-is-the-existential-deposit-#:~:text=On%20the%20Polkadot%20network%2C%20an,needed%20to%20conduct%20a%20transaction.). All native tokens e.g. DOT, ACA, aUSD, BTC etc. would have this feature, but it is not enforced upon ERC-20 tokens in the EVM.

