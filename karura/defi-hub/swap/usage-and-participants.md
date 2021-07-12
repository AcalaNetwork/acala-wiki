# Usage & Participants

## Liquidity Provider

Liquidity providers \(LPs\) deposit their tokens into individual trading pools to gain returns from accumulative trading fees and additional rewards from stability fee shares and liquidity mining programs. There are various risks associated with providing liquidity, which can be found [here](lp-returns-and-risks.md). 

## Traders

This could range from users who want to swap tokens, arbitrage bots to profit from exchange rate differences amongst various platforms, the stablecoin protocol using Swap for liquidation, the network use Swap to settle fee payments, and other protocols usages. 

### **Stablecoin Protocol Liquidation**

The Karura Stablecoin protocol uses the Karura Swap in addition to the auction mechanism for collateral liquidations. Liquidating on Swap is more efficient especially when price fluctuates sharply in a short period of time. The Stablecoin protocol takes into account price slippage, and uses an additional price oracle for ‘fair market price’ before performing an liquidation on the Swap. Since the Swap contributes to the stability of the Stablecoin protocol, a portion of the stability fees are awarded to the liquidity providers based on their shares of relevant pools. 

### **Network Fee Swaps**

The Bring Your Own Gas \(BYOG\) feature of the Karura network allows users to pay transaction fees in any supported tokens. Ultimately these tokens are settled for KAR using the Karura Swap. 

