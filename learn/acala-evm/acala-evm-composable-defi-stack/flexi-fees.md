# Flexible Fees

## **Pay Gas Fees in Virtually Any Token**

With the Acala EVM, fees can be paid in any accepted tokens, such as ACA, aUSD, DOT, and wrapped BTC. More tokens can be supported as native fee tokens with a simple governance approval. [Proposal Template](../../../karura/governance/new-liquidity-pool/proposal-template)

Behind the scenes, Acala DEX is being used as a unified liquidity pool for settling the fees into the network token, but the experience is completely transparent to users and developers.

For example if an app uses ABC token, as long as there is a ABC/aUSD pair, the Dex will do a small swap of ABC to aUSD then to ACA to pay for the fee at the time of execution of the transaction.