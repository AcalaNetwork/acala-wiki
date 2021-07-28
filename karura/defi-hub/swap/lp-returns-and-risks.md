# LP Returns & Risks

## Returns

Liquidity Providers are incentivized to provide liquidity to various trading pools from the following avenues:

1. \([Source](https://github.com/AcalaNetwork/Acala/blob/master/runtime/karura/src/lib.rs#L1067)\) obtaining fees generated from trades
2. \([Source](https://github.com/AcalaNetwork/Acala/blob/4e6a2b94f5153cd7a09279914366927f357767d5/modules/incentives/src/lib.rs#L201)\) receiving a portion of the stability fees \(in kUSD\) from the stablecoin protocol as the Swap is utilized as a complimentary liquidation mechanism
   * the reward rate `DexSavingRewardRate` is yet to be determined
   * APY: 1/2 \* \(1+DexSavingRewardRate\)^\(60 min \* 24 \* 365\)
3. receiving liquidity mining rewards \(in KAR\) from occasional Liquidity Programs for certain pools

### 1. Trading Fees

Liquidity providers share the trading fees based on their LP shares. Therefore trading volume of a particular pool is a good indicator of trading fees generated. Read more on fees [here](fees.md).

### 2. Stability Fee/Stablecoin Interest Rate

Karura Swap is an integral part of the Stablecoin liquidation mechanism, where a hybrid of auction and decentralized exchange \(DeX\) approach is used to ensure efficiency and effectiveness of liquidating collaterals. Therefore a certain portion of the stability fee is configured to shared with the LPs of kUSD pools e.g. KSM/kUSD pair.

### 3. Liquidity Mining Rewards

Liquidity \(aka Total Value Lock - TVL of Kaura Swap\) is highly essential for usability of a given pool, the better the liquidity, the less slippage, potentially attracting more trading, then generating more trading fees for LPs to form a virtuous circle. LP tokens, as receipts of liquidity contribution, is rather liquid, as LP token holders can easily trade them, transfer them, burn them to withdraw liquidity etc. Therefore to encourage locking more liquidity in a pool, Liquidity Mining incentives are set up for certain pairs \(especially but not exclusively stablecoin pairs\).

Staking LP tokens for an incentivized pool, in other words locking these liquidity for the benefit of the pool users, will be rewarded accordingly. Rewards \(in KAR\) are allocated on a per pool basis calculated and released periodically, liquidity providers will receive mining rewards based on their LP shares. 

#### Loyalty Bonus

We implement a Loyalty Bonus mechanism to achieve the following

* To avoid hot bags mining and dumping, hence encouraging more sustainable, longer term farming for the benefit of the protocol and the entire network
* To share more rewards for longer term LPs compared to just vesting

Rewards are accumulated over time, LPs can claim rewards at any time. However, if LPs keep the rewards in the pool until the end of the program, a minimum of X% of Royalty Bonus will be awarded to LPs. 

* If some LPs claim rewards earlier than the mining program ends, then their Royalty Bonus will be added back to the reward pool and shared amongst all other LPs
* Overtime, longer term LPs may enjoy higher rewards than their standard rewards

#### Reward Simulation

* Let’s set up a Liquidity Mining Program for Token A-Token B pool: 500,000 KAR for a period of 1 year
* Loyalty Bonus: 10%
* Assuming 
  * LP1 has maintained and staked his/her LP shares at: 1% \(qualifies for 1% of the rewards\)
  * 5% LPs claimed their rewards earlier, and foregoing the bonus to others
  * The simulation below is per month based, but actual calculation on-chain are based on certain number of blocks

![](https://lh5.googleusercontent.com/De8grUCHCxHReiWnpfoO_mi3ehzdrQ6vI-NZsLQEck23owvX_Wx1L0rjmx3WyeuL5QYbROinogWWoT6tzn-yAgQIXKYgabgbBfgXsMgT0PLe85GbREJRmRqx8qh1IsDQYkHdkf2f)

* LP1 would earn minimum 5,000 KAR including Loyalty Bonus if he/she keeps the rewards in the pool till the end of the year
* 5% LPs claim their rewards early and foregone their Loyalty Bonus, so 2,500 KAR \(=500,000\*10%\*5%\) are added to the pool shared by all other LPs
* The longer LP1 holds his/her rewards, the more rewards he/she will receive due to Loyalty Bonus forgone by others added back to the reward pool
  * If LP1 claims the rewards immediately, he/she will forego the 10% bonus, and receives 375 KAR \(= 41,666.67\*1%\*90%\)
  * If LP1 claims the rewards on Month 5, even if he/she foregoes the 10% bonus, he/she will still receive more rewards at 465 KAR
* With other LPs forgoing their Loyalty Bonus, if LP1 holds onto the rewards till the end, LP1 could potentially earn a total 6,650 KAR at the end of the year

This is an overly simplified example, in reality, an LP's LP shares are likely to be fluctuating due to increase or decrease of overall liquidity, hence resulting in higher or lower mining rewards for certain periods.

## Impermanent Loss

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

