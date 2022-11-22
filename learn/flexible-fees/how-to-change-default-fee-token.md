# How to change default fee token

## Default Fee Token

You can check default fee token order on-chain, go to [Polkadot Webapp](https://polkadot.js.org/apps) - Select the chain (Acala or Karura)

Navigate to `Developer` - `Chain state` > `Constants` >`transactionPayment defaultFeeSwapPathList`

![](<../../.gitbook/assets/Screen Shot 2021-08-04 at 9.06.14 PM.png>)

On Acala, the order might be ACA > aUSD > LCDOT > DOT.

If a user has no ACA balance, then aUSD will automatically be used as fee token.

Users can set their next default fee token to other tokens by executing the following transaction:

```
transactionPayment.setAlternativeFeeSwapPath(fee_swap_path)
```
