# LP Returns & Risks

Liquidity Providers are incentivized to provide liquidity to various trading pools from the following avenues:

* obtaining fees generated from trades
* receiving a portion of the stability fees \(in kUSD\) from the stablecoin protocol as the Swap is utilized as a complimentary liquidation mechanism
* receiving liquidity mining rewards \(in KAR\) from occasional Liquidity Programs for certain pools

However, there are various risks of being a liquidity provider especially when there are significant fluctuations in the underlying asset exchange rates, which may result in **LP’s worse off than simply holding the tokens**. 

If the exchange ratio of the underlying tokens diverges from the exchange ratio when the LP provides liquidity, then there’s potential loss compared to just holding the tokens. The bigger the exchange ratio deviation, the bigger the potential loss. Of course, the loss is only realized when LP withdraws their liquidity. And this is called the impermanent loss.

This  [insightful article](https://pintail.medium.com/uniswap-a-good-deal-for-liquidity-providers-104c0b6816f2) has an in-depth analysis on the impermanent loss, and below is an illustration of this:

* a 1.25x exchange rate change results in a 0.6% loss relative to HODL 
* a 1.50x price change results in a 2.0% loss relative to HODL 
* a 1.75x price change results in a 3.8% loss relative to HODL 
* a 2x price change results in a 5.7% loss relative to HODL 
* a 3x price change results in a 13.4% loss relative to HODL 
* a 4x price change results in a 20.0% loss relative to HODL 
* a 5x price change results in a 25.5% loss relative to HODL

In practice, the actual LP gain is a balance of accumulated fees from each trade on the pools, the accumulated stability fee rewards to LPs, and the impermanent loss. 

