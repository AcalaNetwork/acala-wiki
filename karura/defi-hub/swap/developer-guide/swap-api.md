# Polkadot/Acala API

## Source Code of Karura Dex

[https://github.com/AcalaNetwork/Acala/tree/master/modules/dex](https://github.com/AcalaNetwork/Acala/tree/master/modules/dex)

## Types

Here we will list types and their representation in Typescript.

### **CurrencyId**

```text
type CurrencyId {
    TOKEN: string
}
```

_Example_:

> const acaCurrencyId = { TOKEN: ‘KAR’ }

### **TradingPair**

```text
type TradingPair = [CurrencyId, CurrencyId]
```

_Example_:

> const tradingPair = \[{ TOKEN: ‘KAR’ }, { TOKEN: ‘KUSD’ }\];

### **TradingPairStatus**

```text
type TradingPairStatus = {
    Enabled?: null,
    NotEnabled?: null,
}
```

Indicates whether trading pair is enabled:

* `{ Enabled: null }` - enabled
* `{ NotEnabled: null }` - not enabled

## Initialising Polkadot.js API Provider with Acala wrapper

You can use `@acala-network/api` to get metadata description of available methods.  
 Setting up polkadot/api provider example:

```text
    const provider = new WsProvider('<NODE_WS_ADDRESS>');
    const api = new ApiPromise( options({ provider }) );
    await api.isReady;
    ...
```

You can check out a list of available public endpoints [here](https://hackmd.io/PUA_ova-T9upS9kwnmrP8Q)

**Full code snippet:**

[dex-examples/getPolkadotApi.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getPolkadotApi.js)

## Read Only Functions \(State Queries\)

These functions just read information from the chain, and thus don’t require signing transactions with the private key. Read more about state queries here: [State queries docs](https://polkadot.js.org/docs/api/start/api.query)

### Get system parameters

Such parameters as tokens decimals, symbols, risk parameters and many more can be received from on-chain.

```text
const properties = await api.rpc.system.properties();
const decimals = !result.tokenDecimals.isNone && 
    result.tokenDecimals.value.toHuman();
const symbols = !result.tokenSymbol.isNone && 
    result.tokenSymbol.value.toHuman();
...
```

**Full code snippet:**

[dex-examples/getSystemParameters.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getSystemParameters.js)

### getLiquidity

Returns liquidity of the pool of currencies in Trading Pair `tokenA` and `tokenB`.

```text
liquidityPool(tradingPair: TradingPair): 
    [Balance, Balance] 
```

> Read about `Balance` type [here](https://polkadot.js.org/docs/api/start/typescript/#storage-generics)

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| tradingPair | TradingPair | the tuple of CurrencyIds included in the desired liquidity pool |

> Note ![:warning:](https://assets.hackmd.io/build/emojify.js/dist/images/basic/warning.png): the order of CurrencyIds in the trading pair matters, if it’s wrong, two empty balance will be returned.

Example:

```text
    const liquidity = await api.query.dex.liquidityPool([
        { TOKEN: 'KUSD' },
        { TOKEN: 'KAR' },
    ]);
    console.log(liquidity.map(t => t.toHuman()))
```

**Full code snippet:**

[dex-examples/getLiquidity.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getLiquidity.js)

### getProvisioningPoolBalance

Provisioning Pool is the Pool in the stage of bootstrapping liquidity, when trading is yet allowed. You can read more about [Bootstrap a Pools docs](https://wiki.acala.network/karura/defi-hub/swap/protocol-overview#bootstrap-a-pool) to find out more.

getProvisioningPoolBalance returns a tuple of balances for each of the tokens in the trading pair, depends on the amount of LP tokens that the account is holding

```text
provisioningPool(tradingPair: TradingPair, accountId: string):
[Balance, Balance];
```

| Name | Type |  |
| :--- | :--- | :--- |
| tradingPair | TradingPair | the tuple of currency Ids in provisioning pair |
| accountId | string | the account number balances for |

Example:

```text
    const liquidity = await api.query.dex.provisioningPool([
            { TOKEN: 'KAR' },
            { TOKEN: 'KUSD' }
        ], 
        't98jaBc3cdvZuQpBoiXpJW1uGsFhf9Gq6YDW4UmMtxdxZVL',
    );
```

**Full code snippet:**

[dex-examples/getProvisioningLiquidity.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getProvisioningLiquidity.js)

### tradingPairStatuses

returns if a trading pair is enabled or not. Note that the order of tokens in trading pair is important

```text
tradingPairStatuses(TradingPair): TradingPairStatus
```

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| tradingPair | TradingPair | the tuple of currency Ids in desired trading pair |

Example:

```text
const status = await api.query.dex.tradingPairStatuses([
        { TOKEN: 'KAR' },
        { TOKEN: 'KUSD' }
    ]);
```

**Full code snippet:**

[dex-examples/getTradingPairStatuses.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getTradingPairStatuses.js)

## State-Changing Functions

These transactions write data on-chain and require the private key to sign transactions.  
 There are different ways to create the signer to sign transactions, you can check them in [polkadot api docs](https://polkadot.js.org/docs/api).

Here is an example deriving a `signer` from a seed phrase using Polkadot keyring:

```text
const keyring = new Keyring({
        type: 'sr25519'
    });
const signer = keyring.addFromMnemonic('<YOUR_SEED_PHRASE>');
```

**Full code snippet:**

[dex-examples/getSigner.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/getSigner.js)

### swapWithExactSupply

```text
swapWithExactSupply(path: CurrencyId[], supply_amount: number, min_target_amount: number): Extrinsic
```

Swaps an exact amount of token A `supply_amount` to a minimum amount of token B  
 `min_target_amount`. 

The slippage of the deal = `supply_amount / min_target_amount` 

The current exchange ratio of the Liquidity Pool = `token_A_liquidity / token_B_liquidity`.

Returns `Extrinsic` type that should be signed with the private key.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| path | CurrencyId\[\] | an array of CurrencyIds. The first currency in the path array will be exchanged for the last one. If tokens you want to swap don’t have one pool, the path should contain an intermediate token with which each of the tokens has a pool, and etc. |
| supply\_amount | number | exact supply that will be charged if the swap is successful |
| min\_target\_amount | number | minimum amount to receive if the swap is successful. Changing this amount will define the slippage. |

Example

```text
    const path = [
        { TOKEN: "KAR", },
        { TOKEN: "KUSD", },
    ]
    // minTargetAmount is "0x0" which means that slippage is 100%
    const minTargetAmount = "0x0";
    const supplyAmount = <AMOUNT_DENORMALISED>;

    const extrinsic = api.tx.dex.swapWithExactSupply(
        path,
        supplyAmount,
        minTargetAmount
    );
    const hash = await extrinsic.signAndSend(signer);
    console.log('hash', hash.toHuman());
```

> Note ![:warning:](https://assets.hackmd.io/build/emojify.js/dist/images/basic/warning.png) the supply amount should be denormalised with KAR decimals for this example

**Full code snippet:**

[dex-examples/swapWithExactSupply.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/swapWithExactSupply.js)

### swapWithExactTarget

```text
swapWithExactTarget(path: CurrencyId[], target_amount: number, max_supply_amount: number): Extrinsic
```

Swaps a certain amount of supply tokens A to get an exact amount of tokens B. If the current ratio of tokens in the pool doesn’t need more supply tokens than mentioned, the transaction will fail. 

The slippage of the deal is defined as such:`max_supply_amount / target_amount` 

The current exchange ratio of the Liquidity Pool = `token_A_liquidity / token_B_liquidity`

Returns `Extrinsic` type that should be signed with the private key.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| path | CurrencyId\[\] | An array of CurrencyIds. The first currency in the path array will be exchanged for the last one. If tokens you want to swap don’t have one pool, the path should contain an intermediate token with which each of the tokens has a pool, and etc. |
| target\_amount | number | exact target amount that will be received if the swap is successful |
| max\_supply\_amount | number | The maximum amount that is allowed to spend to get the exact target amount. Variating this argument will define the slippage. |

**Example**:

```text
    const targetAmount = <DENORMALISED_EXACT_AMOUNT>;

    const path = [
        { TOKEN: "KAR", },
        { TOKEN: "KUSD", },
    ]
    const maxSupplyAmount = <DENORMALISED_MAX_AMOUNT>;

    const extrinsic = api.tx.dex.swapWithExactTarget(
        path,
        targetAmount,
        maxSupplyAmount,
    );
    const hash = await extrinsic.signAndSend(signer);
```

**Full code snippet:**

[dex-examples/swapWithExactTarget.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/swapWithExactTarget.js)

### addLiquidity

```text
addLiquidity(
    currency_id_a: CurrencyId, 
    currency_id_b: CurrencyId, 
    max_amount_a: number, 
    max_amount_b: number, 
    min_share_increment: number, 
    stake_increment_share: boolean
): Extrinsic
```

Adds liquidity to the pool of trading pairs `currency_id_a` and `currency_id_b`. The operation will be successful only in case if the token pair is enabled.

You can set the maximum amounts `max_amount_a` and `max_amount_b.` If the ratio of them you provided is not aligned with the current pool ratio, one of the tokens will be charged as max amount, and another adjusted based on the current pool ratio.

`min_share_increment` defines the minimum amount of LP-tokens to receive. This to some degree provides a protective ceiling for potential exploits e.g. front-running.

Returns `Extrinsic` type that should be signed with the private key.

> For example current pool ratio is 0.5 which means that amount of token A in the pool is twice less that token B. If you provide 100 `max_amount_a` and 100 `max_amount_b`, you will add liquidity with 100 Token B and only 50 Token A regarding the currenct pool ratio.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currency\_id\_a | CurrencyId | the first currency of trading pair to provide liquidity to |
| currency\_id\_b | CurrencyId | second currency of trading pair to provide liquidity to |
| max\_amount\_a | number | the denormalized maximum amount of token A in trading pair |
| max\_amount\_b | number | the denormalized maximum amount of token B in traning pair |
| min\_share\_increment | number | the minimum amount of LP tokens you will be received. |
| stake\_increment\_share | boolean | if `true` it automatically stakes all shares |

**Example**

```text
    const currency_id_a = { TOKEN: 'KAR' };
    const currency_id_b = { TOKEN: 'KUSD' };
    const max_amount_a = 2 * 10 ** symbolsDecimals["KAR"];
    const max_amount_b = 2 * 10 ** symbolsDecimals["KUSD"];
    // slippage is 100% for receiving shares
    const min_share_increment = 0;
    const stake_increment_share = true

    const extrinsic = api.tx.dex.addLiquidity(
        currency_id_a,
        currency_id_b,
        max_amount_a,
        max_amount_b,
        min_share_increment,
        stake_increment_share,
    );
    const hash = await extrinsic.signAndSend(signer);
```

**Full code snippet**  
 [dex-examples/addLiquidity.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/addLiquidity.js)

> ![:warning:](https://assets.hackmd.io/build/emojify.js/dist/images/basic/warning.png) Ensure you have positive balance of each of the tokens in the pair and that the pair is available

### removeLiquidity

```text
removeLiquidity(
    currency_id_a: CurrencyId, 
    currency_id_b: CurrencyId, 
    remove_share: number, 
    min_withdrawn_a: number, 
    min_withdrawn_b: number, 
    by_unstake: boolean
): Extrinsic
```

Removes liquidity from selected trading pair. It can, also, automatically unstake all LP tokens and withdraw them. This call merges/batches the two transactions into one and requires only one fee. 

`min_withdrawn_` specifies the minimum amount of token A you want to receive.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currency\_id\_a | CurrencyId | the first currency of trading pair to provide liquidity to |
| currency\_id\_b | CurrencyId | second currency of trading pair to provide liquidity to |
| remove\_share | number | amount of shares to remove liquidity |
| min\_withdrawn\_a | number | the minimum amount of tokens A from the trading pair you want to receive for the specified amount of shares, if the current ratio doesn’t have it, the transaction will fail |
| min\_withdrawn\_B | number | the minimum amount of tokens B you want to receive for the specified amount of shares, if the currenct ratio doesn’t have it, the transaction will fail |
| by\_unstake | boolean | if `true` it will unstake the specified amount of LP-shares and withdraw them. If `false` it will only withdraw unstaked LP tokens |

**Example**

```text
    const currency_id_a = { TOKEN: 'KAR' };
    const currency_id_b = { TOKEN: 'KSM' };
    // amount of shares to remove
    const remove_share = 1;
    // slippage for KAR is 100%, we want to receive anything that liquidity pool ratio will offer
    const min_withdrawn_a = 0;
    // slippage for KSM is 100%, we want to receive anything that liquidity pool ratio will offer
    const min_withdrawn_b = 0;
    const by_unstake = true;

    const extrinsic = api.tx.dex.removeLiquidity(
        currency_id_a,
        currency_id_b,
        remove_share,
        min_withdrawn_a,
        min_withdrawn_b,
        by_unstake
    );
```

**Full code snippet**  
 [dex-examples/removeLiquidity.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/dex-examples/removeLiquidity.js)

