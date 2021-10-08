# Liquid Staking

Karura’s Liquid Staking protocol is to solve the illiquidity challenge of staked assets.The protocol establishes a staking pool, where users can stake KSM and mint LKSM, which is Kusama staking yield-bearing, while fungible, tradable and usable in other protocols and parachains. Read more [here](https://wiki.acala.network/karura/defi-hub/liquid-staking/lksm)

Follow the roll-out plan of the Liquid Staking protocol [here](https://wiki.acala.network/karura/defi-hub/liquid-staking/protocol-overview#rollout-roadmap)

To interact with Karura from Javascript you can use `@polkadot/api` along with `@acala-network/api`. You can learn more about `@polkadot/api` \[here\]. \([https://polkadot.js.org/docs/api](https://polkadot.js.org/docs/api)\).

The [Liquid Staking SDK](https://github.com/AcalaNetwork/acala.js/tree/master/packages/sdk-homa) provides more convenient methods around Liquid staking protocol.

## Source Code

[https://github.com/AcalaNetwork/Acala/tree/master/modules/homa-lite](https://github.com/AcalaNetwork/Acala/tree/master/modules/homa-lite)

## Read-Only Functions \(State queries\)

These functions only read information from the chain, and thus don't require signing transactions with a private key. Read more about state queries here: [State queries docs](https://polkadot.js.org/docs/api/start/api.query)

### Get the maximum amount of tokens that can be staked

The protocol has an arbitrary staking cap to control how much KSM is accepted into the staking pool.

You can check it with method:

```text
    homaLite.stakingCurrencyMintCap(): Promise<Balance>
```

Example:

```text
    const result = await api.query.homaLite.stakingCurrencyMintCap();
    console.log(result.toHuman());
```

### Get total staked in protocol amount

Returns `Balance` type with total currency in the staking pool.

```text
    homaLite.totalStakingCurrency(): Promise<Balance>
```

Example:

```text
    const result = await api.query.homaLite.totalStakingCurrency();
    console.log(result.toHuman());
```

### Get Minimum Mint Threshold

Minimum amount of staking asset that is required, has type `Balance` which has 12 decimals.

```text
    minimumMintThreshold(): Promise<Balance>
```

Example:

```text
    const result = await api.const.homaLite.minimumMintThreshold();
    console.log(result.toHuman());
```

### Get Minting fee per one operation

Returns the amount of fee that users pay per one staking operation, no matter how big is the stake.

```text
    mintFee(): Promise<Balance>
```

Example:

```text
    const result = await api.const.homaLite.mintFee();
    console.log(result.toHuman());
```

### Get Staking Pool Account in Kusama

Karura's parachain account on Kusama that is used for managing staking assets. This account has no private key and is trustlessly controlled by the parachain.

```text
    sovereignSubAccountLocation(): Promise<Balance>
```

Example:

```text
    const result = await api.const.homaLite.sovereignSubAccountLocation();
    console.log(result.toHuman());
```

## State-Changing Functions

These transactions updates on-chain data and require sending transactions to the blockchain.

To perform run test code snippets ensure that you have `SEED_PHRASE` environment variable defined in your `.env` file.

### Mint LKSM

To mint LKSM you need to provide the amount of KSM.

Fees include minting fee and inter-chain transaction fee.

LKSM is a share representation of the staking pool which entitles the owner to a likely increasing quantity of underlying assets.

```text
mint(amount: balance): Extrinsic
```

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| amount | Balance | amount of currency to stake |

Example

```text
  const amount = <DESIRED_AMOUNT>

  const extrinsic = api.tx.homaLite.mint(
    amount,
  );
  const hash = await extrinsic.signAndSend(signer);
  console.log('hash', hash.toHuman());
```

### Redeem Staking currency

Soon to come...

