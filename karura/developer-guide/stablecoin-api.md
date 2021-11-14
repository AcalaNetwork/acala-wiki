# Stablecoin

Karura Dollar \(kUSD\) stablecoin protocol is a dynamic decentralized system that allows users to autonomously generate kUSD stablecoin \(stabilized against the US Dollar\) using crypto assets in excess of value as collateral.

To interact with Karura from Javascript you can use `@polkadot/api` along with `@acala-network/api`. You can learn more about `@polkadot/api` \[here\]. \([https://polkadot.js.org/docs/api](https://polkadot.js.org/docs/api)\).

We do also provide a [Stablecoin SDK](https://github.com/AcalaNetwork/acala.js/tree/master/packages/sdk-loan) which provides more some automation around stablecoins.

## Source Code of Karura Stablecoin

[https://github.com/AcalaNetwork/Acala/tree/master/modules/honzon](https://github.com/AcalaNetwork/Acala/tree/master/modules/honzon)

## Read-Only Functions \(State queries\)

These functions only read information from the chain, and thus don't require signing transactions with a private key. Read more about state queries here: [State queries docs](https://polkadot.js.org/docs/api/start/api.query)

### Get Vault for specific Account for given Collateral Type

Returns amount of `collateral` and amount of minted stablecoin as `debit` for specific collateral type and account.

> Note :warning: `debit` reflects the only amount of minted kUSD. The amount of debt is higher as it includes accumulated interest. To calculate the total amount to payback you need to use `debitExchangeRate` parameter \(the example for fetching `debitExchangeRate` is shown below\).

```typescript
positions(currencyId: CurrencyId, accountId: AccountId):
    Promise<{collateral: number, debit: number}>
```

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | collateral currency Id |
| accountId | AccountId | account to fetch vaults for, can be passed as string |

Example:

```typescript
    const result = await api.query.loans.positions(
        { TOKEN: "KSM" },
        "<ACCOUNT>"
    );
  console.log(result.toHuman());
```

#### Full code snippet:

[loan-examples/get-positions.js](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/loan-examples/get-positions.ts)

### Get total amount of collateral and borrowed kUSD for Collateral Type

Returns total amount of `collateral` and amount of borrowed stablecoin as `debit` for specific collateral type.

```typescript
totalPositions(currencyId: CurrencyId):
    Promise<{collateral: number, debit: number}>
```

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | identificator for currency used as collateral |

Example:

```typescript
    const result = await api.query.loans.totalPositions(
        { TOKEN: "KSM" }
    );
  console.log(result.toHuman());
```

#### Full code snippet:

[loan-examples/total-positions.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/loan-examples/total-positions.ts)

### Get Risk Parameters for given Collateral type.

Each accepted by Karura Collateral Type can have different risk parameters. These values are controlled by Karura Governance.

```typescript
collateralParams(currencyId: CurrencyId):
    Promise<{
    maximumTotalDebitValue: number,
    interestRatePerSec: number, 
    liquidationRatio: number,
    liquidationPenalty: number,
    requiredCollateralRatio: number,

    }>
```

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | identificator for currency used as collateral |

**Return values**

| Name | Type |  |
| :--- | :--- | :--- |
| maximumTotalDebitValue | Number | maximum amount of KUSD that can be borrowed for one vault |
| interestRatePerSec | Percent | the percentage of borrowed kUSD to be paid each next block. This amount should be added to `globalInterestRatePerSec` to calculate the debt |
| liquidationRatio | Percent | collateral ratio \(collateral value / debt value\) reaching which the vault gets liquidated |
| liquidationPenalty | Percent | penalty that is charged from the vault if the vault gets liquidated |
| requiredCollateralRatio | Percent | Minimum collateral ratio till which user can borrow kUSD |

Example:

```typestript
    const result = await api.query.cdpEngine.collateralParams({ 
      TOKEN: "KSM" 
    });
    console.log(result.toHuman());
```

#### Full code snippet:

[loan-examples/collateral-params.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/loan-examples/collateral-params.ts)

### Get Debt Exchange Rate for given collateral type

This parameter is used to calculate the debt. The amount of minted kUSD should be multiplied by this parameter. As the Interest rate is accumulated depends on block number, it makes sense to fetch this parameter for a certain block.

```typescript
debitExchangeRate(currencyId: CurrencyId):
    Promise<number}>
```

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | identificator for currency used as collateral |

Example:

```text
  const result = await api.query.cdpEngine.debitExchangeRate.at(
      '<BLOCK_HASH'
      { TOKEN: "KSM" }
  );
  console.log(result.toHuman());
```

#### Full code snippet:

[loan-examples/debit-exchange-rate.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/loan-examples/debit-exchange-rate.ts)

## State-Changing Functions

These transactions write data on-chain and require a private key to sign the transaction. To perform run test code snippets ensure that you have `SEED_PHRASE` environment variable defined in your `.env` file.

### Create and manage the Vault

All operations: creating a vault, adding/removing collateral, borrowing, paying back kUSD can be done using a single method: `honzon.adjustLoan`

```typescript
adjustLoan(currency_id: CurrencyId, collateral_adjustment: Number, debit_adjustment: Number): Extrinsic
```

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | collateral CurrencyId |
| collateral\_adjustment | Signed Amount | positive means to deposit collateral currency into Vault, negative means withdraw collateral currency from the Vault |
| debit\_adjustment | Signed Amount | positive means to mint some amount of stablecoin to the caller; negative means that caller will pay back stablecoin to the Vault |

Example

```typescript
  const currencyId = { TOKEN: "KSM" };
  const collateralAdjustment = <DESIRED_ADJUSTMENT>;
  const debitAdjustment = <DESIRED_ADJUSTMENT>;

  const extrinsic = api.tx.honzon.adjustLoan(
    currencyId,
    collateralAdjustment,
    debitAdjustment
  );
  const hash = await extrinsic.signAndSend(signer);
  console.log('hash', hash.toHuman());
```

> Note :warning: the supply amount should be denormalised with KAR decimals for this example

#### Full code snippet:

[loan-examples/adjustLoan.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/loan-examples/adjustLoan.ts)

### Grants permission for transferring loan

Sets permission to transfer caller's loan to another Account \(`to`\).

```typescript
authorize(curencyId: CurrencyId, to: AccountId): Extrinsic
```

Returns `Extrinsic` type that should be signed with a private key.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | collateral CurrencyId |
| to | AccountId | sets permission to transfer loan for this accountId |

**Example**:

```typescript
  const accountId = "<ACCOUNT_ID>";
  const extrinsic = api.tx.honzon.authorize(
    { TOKEN: "KSM" }, 
    accountId
  );
  const hash = await extrinsic.signAndSend(signer);
  console.log("hash", hash.toHuman());
```

#### Full code snippet:

[loan-examples/authorize.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/loan-examples/authorize.ts)

### Removes permission for transferring loan

Removes permission to transfer caller's loan to another Account \(`to`\). This method can be used to decline previously given permission.

```typescript
unauthorize(curencyId: CurrencyId, to: AccountId): Extrinsic
```

Returns `Extrinsic` type that should be signed with a private key.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | collateral CurrencyId |
| to | AccountId | removes permission to transfer loan for this accountId |

**Example**:

```typescript
  const accountId = "<ACCOUNT_ID>";
  const extrinsic = api.tx.honzon.unauthorize(
    { TOKEN: "KSM" }, 
    accountId
  );
  const hash = await extrinsic.signAndSend(signer);
  console.log("hash", hash.toHuman());
```

### Removes permissions for ALL accounts to transfer the vault

Removes permission to transfer caller's vault to ALL accounts for all Collateral types. This method can be used to decline previously given permission.

```text
unauthorizeAll(): Extrinsic
```

Returns `Extrinsic` type that should be signed with a private key.

**Example**:

```typescript
  const extrinsic = api.tx.honzon.unauthorizeAll();
  const hash = await extrinsic.signAndSend(signer);
  console.log("hash", hash.toHuman());
```

### Transfering Vault to another account

Transfers Vault to the caller's account if it has permissions \(if `authorize` was called previously by `from` account\).

```text
transferLoanFrom(currency_id, from): Extrinsic
```

Returns `Extrinsic` type that should be signed with a private key.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | collateral CurrencyId |
| from | AccountId | transfers collateral from this account to the caller |

**Example**:

```typescript
    const fromAccountId = "<ACCOUNT_ID>";
    const extrinsic = api.tx.honzon.transferLoanFrom(
      { TOKEN: "KSM" },
      fromAccountId
    );
    const hash = await extrinsic.signAndSend(signer);
    console.log("hash", hash.toHuman());
```

### Closing caller's Vault by swapping collateral in DeX

This action can be done with `adjustLoan`, but there is a shortcut created for this purpose which is applied only to safe vaults \(where the collateral ratio is above liquidation level\) and where the debt amount is positive.

This method closes the caller's Vault by selling a sufficient amount of collateral on Karura Dex.

```typescript
closeLoanHasDebitByDex(
    currency_id: CurrencyId, 
    max_collateral_amount: number, 
    maybe_path?: CurrencyId[]
): Extrinsic
```

Returns `Extrinsic` type that should be signed with a private key.

**Arguments**

| Name | Type |  |
| :--- | :--- | :--- |
| currencyId | CurrencyId | collateral CurrencyId |
| max\_collateral\_amount | number | the maximum collateral that's allowed to be swapped in DeX to pay back the Vault |
| maybe\_path | CurrencyId\[\] | swap path that can be used for swapping collateral for kUSD in DeX |

**Example**:

```typescript
    const extrinsic = api.tx.honzon.closeLoanHasDebitByDex(
      { TOKEN: "KSM" },
      // large number, allows swapping almost any amount
      1 * 10 ** 30,
      [{ TOKEN: "KSM" }, { TOKEN: "KUSD" }]
    );
    const hash = await extrinsic.signAndSend(signer);
    console.log("hash", hash.toHuman());
```

#### Full code snippet:

[loan-examples/close-vault-with-dex.ts](https://github.com/AcalaNetwork/acala-js-example/blob/master/src/loan-examples/close-vault-with-dex.ts)

