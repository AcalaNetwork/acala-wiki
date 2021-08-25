# New Liquidity Pool

The Karura Swap protocol can support multiple liquidity pools each of which is made up of two different tokens e.g. Token A-Token B pool and Token B-Token C pool. Users can swap any token to any other token in a single transaction provided there is a path between them e.g. even if there's no Token A-Token C pool, users can still swap Token A for Token C, given they have a common path via Token B.  

## **1. Proposing a new Liquidity Pool**

As for any governance upgrades, a referendum proposal process is recommended to add a new pair and open a new liquidity pool. The proposal and subsequent discussion should cover the following \(you can use the [proposal template](https://docs.google.com/document/d/1Q1KTW3t5-ldjsTOnli22QmE7mHnguwPy9QyueNfnUAA/edit#heading=h.6usht62z8wr4) here\). Use the [Karura Community Forum](https://acala.discourse.group/c/karura/new-liquidity-pool/23) to gather sentiment and feedback

* Motivation
* Evaluation of Liquidity Pool
  * Trading pair
  * Motivations for creating the pool
  * Mechanisms for listing new pool: direct listing, [Bootstrap](https://wiki.acala.network/karura/defi-hub/swap/bootstrap-a-pool) or else
    * List Bootstrap parameters if applicable
  * Liquidity Program if any
* Evaluation of the tokens 
  * Utility of the token
  * Features and innovation of the protocol, product, blockchain behind the token
  * Token distribution and vesting schedule
  * Token economics
  * Project information
* Risks and disclaimers
* Community Sentiment
  * Liquidity Plan
  * Mechanisms for listing new pool: direct listing, Bootstrap or else
  * Every pool requires both sides of the token pair as liquidity, please provide a plan on how to bootstrap liquidity

## **2. Prepare the payload**

Your governance on-chain proposal must include the necessary call data. Please follow this guide here. 

## **3. Submission of Proposal**

The Proposal can now be submitted on-chain. The proposer must have enough a minimum deposit of 100 KAR to propose a referendum, or have a council member to submit it on behalf.

