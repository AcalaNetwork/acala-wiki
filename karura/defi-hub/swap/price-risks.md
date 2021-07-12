# Price Risks

While Karura Swap provides a simplistic and elegant approach to generate a trustless and automatic on-chain exchange rate for two tokens, **it is prone to manipulation when it is used as the sole source of price information**. 

When a protocol/program \(smart contract or runtime module\) wants to swap X Token A for as much Token B as possible, relying purely on the Swap exchange rate will make it vulnerable to frontrunning which may result in economic loss. A malicious actor with the knowledge of this trade, could arbitrarily push the price up before the trade goes through, then wait for the trade to execute, then push the price back down to profit from it. 

Using an additional price oracle as a reference to ‘fair’ or ‘true’ market rate would mitigate this risk to a large extent. Users can also specify a slippage tolerance to ensure price only changes within an acceptable interval e.g. 0.5%. Highly liquid pools with low-value trades would generally have low slippage. 

