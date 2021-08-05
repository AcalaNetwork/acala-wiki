# Swap SDK

Swap SDK is meant to provide simple interfaces for calculating all parameters needed for token swaps: slippage, price impact, exact supply/target, etc.

Before making any swap there needed to few calls to check liquidity and to calculate some basic parameters to make swaps safer and to avoid malicious operations as front-running or sandwich-attacks.

First, you need to create an instance of either SwapPromise or SwapRx:

```text
  const swapPromise = new SwapPromise(api);
```

After setup arguments of swap:

```text
  const karToken = walletPromise.getToken("KAR");
  const kusdToken = walletPromise.getToken("KUSD");

  const path = [karToken, kusdToken] as [Token, Token];
  const supplyAmount = new FixedPointNumber(1, karToken.decimal);
  // set slippage 1 %
  const slippage = new FixedPointNumber(0.01);
```

And make calculate parameters with next interface:

```text
const parameters = swapPromise.swap(path: Token[], amount: FixedPointNumber, type: "EXACT_INPUT" | "EXACT_OUTPUT");
```

`type` defines whether the swap should be with exact supply or exact target.

You can find the full code sample here: [swapWithSDK.ts](https://github.com/AcalaNetwork/acala-js-example/blob/c85aff894ec3aa083aea11ec80e0a985adb5fa04/src/dex-examples/swapWithSDK.ts)

