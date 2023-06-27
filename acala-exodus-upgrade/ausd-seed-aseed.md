# aUSD SEED (aSEED)

The goal of aUSD Seed (aSEED) is to provide a pathforward for aUSD with options to exit existing aUSD holdings/vaults or to participate in Acala’s future growth.

## aUSD Conversion

aUSD will be converted to aSEED 1:1 across all avenues including account balance, and liquidity pools etc. This means current aUSD holders will hold aSEED instead, and aUSD LPs will be converted to aSEED LPs.

Preparation: prior to aUSD to aSEED conversion, set Honzon risk parameters to stop minting, liquidation etc.

aUSD (Karura) will be converted to aSEED (Karura), aUSD (Acala) will be converted to aSEED (Acala).

### aUSD Conversion Date

aUSD Conversion is targeted to be on July 20 (exact block TBD) provided that the community vote is passed and on-chain changes are executed. From this date, `aUSD` will become `aSEED` (aUSD Seed).

The on-chain `assetRegistry` will be updated with the new symbol (aSEED) and name (aUSD SEED). &#x20;

If you are a holder of aUSD, then you do not need to take any action.

If you are a builder of a tool that consumes `@acala-network/api` then there should be no real changes to be made in your application. However if your application displays the token symbol and token name in an offchain way, then you will need to ensure you display the correct symbol and name.

## CDP Conversion

For a CDP owner who has borrowed $$x$$ amount of aUSD with a deposit of $$y_{c_i}$$ amount of collateral type $$c_i$$, and chooses not to repay their CDP debt by the aSEED conversion event:

At the aSEED conversion

* their $$x$$ amount of aUSD tokens will become $$x$$  amount of aSEED tokens ​
* the amount of collateral going into the aSEED treasury is:

$$
\frac{x \hat{P_a} }{ \hat{P_{c_i}}}
$$

where $$\hat{P_a},  \hat{P_{c_i}}$$ are the aSEED conversion prices of aUSD and collateral type $$c_i$$ passed at the community votes (for Acala see the vote [here](https://voting.opensquare.io/space/acala/proposal/QmXFw8DZbX5wDFeD1kQtFDy8tmE4FKEir7tZVqe9vCqBTb), for Karura see the vote [here](https://voting.opensquare.io/space/karura/proposal/QmUuHgFt4fN4iKU6JzW2utx2cykz4Er3EyhLHRwYEjDk3r))

* the amount of collateral returning to the CDP owner is:&#x20;

$$
y_{c_i} - \frac{x \hat{P_a} }{ \hat{P_{c_i}}}
$$

### Example

For a CDP owner who has borrowed 200 aUSD with a deposit of 100 DOTs, and chooses not to repay their CDP debt by the aSEED conversion event: ​&#x20;

At the aSEED conversion, ​&#x20;

* their 100 aUSD tokens will become 100 aSEED tokens
* the amount of DOTs going into the aSEED treasury is:

$$
\frac{200 \cdot 0.538322 }{4.587095} = 23.471151
$$



* the amount of DOTs returning to the CDP owner is:

$$
100 - \frac{200 \cdot 0.538322 }{4.587095} = 76.528848
$$



## Redemption

aSEED holders can redeem the underlying assets in the aSEED treasury in future under certain set criteria, e.g. after 12 months and aSEED underlying value >= $1. The pallet code will be developed and criteria parameters will be voted in via governance.
