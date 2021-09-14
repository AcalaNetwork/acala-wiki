# Liquid staking developer guide (Light)

Karura’s Liquid Staking protocol is simplified staking for KSM and DOT. The protocol establishes a staking pool, where users can stake KSM and mint LKSM, which is Kusama staking yield-bearing, while fungible, tradable and usable in other protocols and parachains.
The light version of Liquid staking which is described here is a simplified version of full liquid staking protocol, e.g. selecting validators is done by Acala Foundation when in the full Liquid Staking protocol this ability is managed by all network token holders.

To interact with Karura from Javascript you can use `@polkadot/api` along with `@acala-network/api`. You can learn more about `@polkadot/api` [here]. (https://polkadot.js.org/docs/api).

We do also provide a [Light Liquid Staking SDK](https://github.com/AcalaNetwork/acala.js/tree/master/packages/sdk-homa) which provides more some automation around Liquid staking protocol.

## Source Code
https://github.com/AcalaNetwork/Acala/tree/master/modules/homa-lite

## Read-Only Functions (State queries)

These functions only read information from the chain, and thus don't require signing transactions with a private key. Read more about state queries here: [State queries docs](https://polkadot.js.org/docs/api/start/api.query)


### Get the maximum amount of tokens that can be staked

There is a maximum amount of each token (KSM or DOT) that can be accepted in total. 
You can check it with method:

```typescript=
    homaLite.stakingCurrencyMintCap(): Promise<Balance>
```

Example:
```typescript=
    const result = await api.query.homaLite.stakingCurrencyMintCap();
    console.log(result.toHuman());
```

### Get total staked in protocol amount

Returns `Balance` type with total currency in the staking pool.

```typescript=
    homaLite.totalStakingCurrency(): Promise<Balance>
```

Example:
```typescript=
    const result = await api.query.homaLite.totalStakingCurrency();
    console.log(result.toHuman());
```

### Get Minimum Mint Threshold

returns the minimum amount of staking currency that can be staked per one account, has type `Balance` which has 12 decimals.

```typescript=
    minimumMintThreshold(): Promise<Balance>
```

Example:
```typescript=
    const result = await api.const.homaLite.minimumMintThreshold();
    console.log(result.toHuman());
```

### Get Minting fee per one operation

returns the amount of fee that users pay per one staking operation, no matter how big is the stake.


```typescript=
    mintFee(): Promise<Balance>
```

Example:
```typescript=
    const result = await api.const.homaLite.mintFee();
    console.log(result.toHuman());
```

### Get Staking Pool account in Kusama

All funds is collected into one account that stakes everything in Kusama network. The address of the account can be retrieved using this method.


```typescript=
    sovereignSubAccountLocation(): Promise<Balance>
```

Example:
```typescript=
    const result = await api.const.homaLite.sovereignSubAccountLocation();
    console.log(result.toHuman());
```


---
## State-Changing Functions 

These transactions write data on-chain and require a private key to sign the transaction.
To perform run test code snippets ensure that you have `SEED_PHRASE` environment variable defined in your `.env` file.


### Mint LKSM

To mint Liquid currency you need to provide the amount of staking currency that you want to pay. Const of this operation accumulates Minting Fee and Interchain Transaction Fee.
Liquid currency is a redemption token that can be used to withdraw underlying assets, and besides it can be used in a variety of network activities, as collateral for minting KUSD, paying fees, etc.


```typescript=
mint(amount: balance): Extrinsic
```


**Arguments**

| Name | Type |  |
| -------- | -------- | -------- |
| amount     | Balance     | amount of currency to stake |

Example
```typescript=
  const amount = <DESIRED_AMOUNT>

  const extrinsic = api.tx.homaLite.mint(
    amount,
  );
  const hash = await extrinsic.signAndSend(signer);
  console.log('hash', hash.toHuman());
```


### Redeem Staking currency

Soon to come...