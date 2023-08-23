# Acala Liquid Staking with Homa

To interact with Acala or Karura from Javascript you can use `@polkadot/api` along with `@acala-network/api`. 

For those looking to stake DOT or unstake LDOT, we also offer the [Homa SDK](https://github.com/AcalaNetwork/acala.js/tree/master/packages/sdk-homa).


## Homa Example

The following example demonstrates how to utilize the Homa SDK and the Polkadot API for staking DOT and unstaking LDOT. 

You can fork the [live example](https://replit.com/@xlc/acalajs-examples#index.js) on replit and run the example yourself.

{% hint style="warning" %}
To run the example on replit, you neeed to first fork it, otherwise it will only show an empty webview page. 

Actual result will be logged to console instead of the webview page, only after you fork it.
{% endhint %}


### prepare env, api, and sdk
first import deps 
```ts
const { fetchConfig, setupWithServer } = require('@acala-network/chopsticks')
const { ApiPromise, WsProvider } = require('@polkadot/api')
const { Homa, Wallet } = require('@acala-network/sdk')
const { createTestKeyring } = require('@polkadot/keyring')
```

use Chopsticks to fork mainnet to a local testnet
```ts
const acalaConfig = await fetchConfig('acala')
acalaConfig.port = 8111 // set the port
acalaConfig.block = 4286000
acalaConfig.db = './db.sqlite'
const { chain, listenPort, close } = await setupWithServer(acalaConfig)

// This server can be accessed at wss://acalajs-examples--xlc.repl.co/
// Use https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Facalajs-examples--xlc.repl.co to connect to this testnet
console.log('Chopsticks is running on port', listenPort)
```
```
------------------------------------------- console -------------------------------------------
INFO (14701): Loading config file https://raw.githubusercontent.com/AcalaNetwork/chopsticks/master/configs/acala.yml
2023-08-23 14:22:26        REGISTRY: Unknown signed extensions SetEvmOrigin found, treating them as no-effect
Chopsticks is running on port 8111
INFO (rpc/14701): Acala RPC listening on port 8111
```

setup Acala api connect to the local testnet
```ts
const provider = new WsProvider(`ws://localhost:${listenPort}`)
const api = new ApiPromise({ provider, noInitWarn: true })
await api.isReady
```

add a couple helpers
```js
// send a transaction and wait for it to be included
const sendTxAndWait = (tx) => {
  return new Promise((resolve) => {
    tx.send((status) => {
      if (status.isInBlock || status.isFinalized) {
        resolve(status.events)
      }
    })
  })
}

// sign tx with a mock signature that will be accepted by Chopsticks
// so we can act on behalf of any accounts without the private key
// this requires mockSignatureHost to be enabled
const fakeSign = (tx, addr, nonce) => {
  const mockSignature = new Uint8Array(64)
  mockSignature.fill(0xcd)
  mockSignature.set([0xde, 0xad, 0xbe, 0xef])
  tx.signFake(addr, {
    nonce,
    genesisHash: api.genesisHash,
    runtimeVersion: api.runtimeVersion,
    blockHash: api.genesisHash,
  })
  // update fake signature
  tx.signature.set(mockSignature)

  return tx
}
```

setup walelt sdk and homa sdk
```ts
const wallet = new Wallet(api)
await wallet.isReady

const homa = new Homa(api, wallet)
await homa.isReady
```

query some info
```ts
// treasury address
const address = '23M5ttkmR6KcoTAAE6gcmibnKFtVaTP5yxnY8HF1BmrJ2A1i'

console.log('Address', address)

const ldot = { Token: 'LDOT' }

// check LDOT balance
const balance = await api.query.tokens.accounts(address, ldot)
const free = balance.free.toNumber()

console.log('LDOT Balance:', free / 10 ** 10)

// get exchange rate
const exchangeRate = (await homa.getEnv()).exchangeRate.toNumber()
console.log('Exchange Rate:', exchangeRate)

// calculate DOT amount
const dotAmount = free * exchangeRate
console.log('DOT amount:', dotAmount / 10 ** 10)
```
```
------------------------------------------- console -------------------------------------------
Address 23M5ttkmR6KcoTAAE6gcmibnKFtVaTP5yxnY8HF1BmrJ2A1i
LDOT Balance: 21.1962713105
Exchange Rate: 0.13037338530710973
DOT amount: 2.7634296466378525
```

### stake DOT
```ts
// Bob
const testaddr = '246gNkjCexYRsCpdjtVhz35sHjcb21jpqipzT9u4uwKV8iEE'

// use Chopsticks to mint this user some ACA and DOT
await api.rpc('dev_setStorage', {
  System: {
    Account: [
      [
        // key
        [testaddr],
        // value
        {
          providers: 2, // 1 for ACA, 1 for DOT
          data: {
            free: 1000 * 1e12 // 1000 ACA
          }
        }
      ]
    ]
  },
  Tokens: {
    Accounts: [
      [
        // key
        [
          testaddr,
          { Token: 'DOT' }
        ],
        // value
        {
          free: 100 * 10e10 // 100 DOT
        }
      ]
    ]
  }
})

{
  // create tx to use 100 DOT to mint LDOT
  const tx = api.tx.homa.mint(100 * 10e10)
  fakeSign(tx, testaddr, 0)

  console.log('\nSend tx to use 100 DOT to mint LDOT')
  const events = await sendTxAndWait(tx)
  console.log('tx events', events.map(x => x.event.toHuman()))
}

const ldotBalance = (await api.query.tokens.accounts(testaddr, ldot)).free.toNumber()
console.log('\nMinted', ldotBalance / 1e10, 'LDOT')
```

```
------------------------------------------- console -------------------------------------------
Send tx to use 100 DOT to mint LDOT
INFO (block-builder/14701): Try building block #4,286,001
    number: 4286001
    extrinsicsCount: 1
    umpCount: 0
tx events [
  <many tx events>
]

Minted 7667.5245218183 LDOT
```

### unstake LDOT
There are number of ways to unstake LDOT / convert it back to DOT
1. Slow unstake by wait for 28 days. 
2. Use fast redeem by matching with new stakers. A fast redeem fee is charged. This is only available if there are DOT in the pending pool.
3. Use the Taiga tDOT pool to swap LDOT to DOT. A swap fee is charged. The swap rate depends on the pool balance

#### slow unstake
```ts
// unstake half 
const unstakeAmount = Math.floor(ldotBalance / 2)

{
  // create tx to unstake without fast redeem. i.e. wait for 28 days unstaking period
  const tx = api.tx.homa.requestRedeem(unstakeAmount, false /* true to allow fast redeem if possible */)
  fakeSign(tx, testaddr, 1)

  console.log('Send tx to unstake', unstakeAmount / 1e10, 'LDOT')
  const events = await sendTxAndWait(tx)
  console.log('tx events', events.map(x => x.event.toHuman()))
}

// the redeem request is still pending and not yet enacted on relaychain
const pendingRedeemLDOT = (await api.query.homa.redeemRequests(testaddr))
console.log('\nPending redeem LDOT amount', pendingRedeemLDOT.toHuman())

const keyring = createTestKeyring()
const sudoKey = keyring.getPair('5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY') // Alice

// simulate relaychain era bump
// this will trigger XCM to do unstake on relaychain
await sendTxAndWait(await api.tx.sudo.sudo(api.tx.homa.forceBumpCurrentEra(1)).signAsync(sudoKey))

const unbondings = await api.query.homa.unbondings.entries(testaddr)
console.log('\nUnbonding requests', unbondings.map(([key, value]) => [key.toHuman(), value.toHuman()]))

// bump 28 era so that the DOT is available for withdraw
await sendTxAndWait(await api.tx.sudo.sudo(api.tx.homa.forceBumpCurrentEra(28)).signAsync(sudoKey))

await sendTxAndWait(fakeSign(api.tx.homa.claimRedemption(testaddr /* any account can trigger redeem */), testaddr, 2))

const dotBalance = (await api.query.tokens.accounts(testaddr, { token: 'DOT'})).free.toNumber()
console.log('\nRedeemed', dotBalance / 1e10, 'DOT')
```

```
------------------------------------------- console -------------------------------------------
Send tx to unstake 3833.7622609091 LDOT
INFO (block-builder/14701): Block built
    number: 4286001
    hash: "0xaaec1a0bc862a76af6b53bcc60c7b62256487cce5c468828360950491d281d7f"
    extrinsics: [
      "0xbd0184008eaf04151687736326c9fea1…cdcdcdcd00000074000b00a0724e1809"
    ]
    pendingExtrinsicsCount: 0
    ump: {}
INFO (block-builder/14701): Try building block #4,286,002
    number: 4286002
    extrinsicsCount: 1
    umpCount: 0
tx events [
  <tx events>
]

Pending redeem LDOT amount [ '38,337,622,609,091', false ]
INFO (block-builder/14701): Block built
    number: 4286002
    hash: "0xed20da2f91d4a5f2d3104c2c25bd3005d965a4837e0ccb90f9bba865b6b0c2fd"
    extrinsics: [
      "0xc10184008eaf04151687736326c9fea1…cdcdcd00040074010bc38c602cde2200"
    ]
    pendingExtrinsicsCount: 0
    ump: {}
INFO (block-builder/14701): Try building block #4,286,003
    number: 4286003
    extrinsicsCount: 1
    umpCount: 0

Unbonding requests [
  [
    [ '246gNkjCexYRsCpdjtVhz35sHjcb21jpqipzT9u4uwKV8iEE', '1,205' ],
    '4,999,981,935,420'
  ]
]
INFO (block-builder/14701): Block built
    number: 4286003
    hash: "0xece387f4671b9245969411be150f01958642c28b8e663c8b374dc24941e361d4"
    extrinsics: [
      "0xbd018400d43593c715fdd31c61141abd…6d6e0a8924010000ff00740801000000"
    ]
    pendingExtrinsicsCount: 0
    ump: {}
INFO (block-builder/14701): Try building block #4,286,004
    number: 4286004
    extrinsicsCount: 1
    umpCount: 0
INFO (block-builder/14701): Block built
    number: 4286004
    hash: "0x6509c89c4bb36e7181a0d0016ec80c74bed40faa967b0179d7c149d0e0212baf"
    extrinsics: [
      "0xbd018400d43593c715fdd31c61141abd…e53ea68134010400ff0074081c000000"
    ]
    pendingExtrinsicsCount: 0
    ump: {}
INFO (block-builder/14701): Try building block #4,286,005
    number: 4286005
    extrinsicsCount: 1
    umpCount: 0

Redeemed 499.998193542 DOT
```

#### swap with taiga tDOT pool 
use aggregatedDex to perform swap, which allow multiple step swap with both Taiga pool and Acala Dex pool in a single tx
```ts
const tx = api.tx.aggregatedDex.swapWithExactSupply(
  [{Taiga: [/*pool id*/ 0, /*supply asset*/1, /*target asset*/0]}],
  unstakeAmount,
  0 // should always set a min target to prevent unexpected result
)
fakeSign(tx, testaddr, 3)

console.log('Send tx to swap', unstakeAmount / 1e10, 'LDOT')
const events = await sendTxAndWait(tx)
console.log('tx events', events.map(x => x.event.toHuman()))
```

```
------------------------------------------- console -------------------------------------------
Send tx to swap 3833.7622609091 LDOT
INFO (block-builder/14701): Block built
    number: 4286005
    hash: "0xf62a24768622c45aca7411c77df101dffb934f2a4d63dfd19dc3ff795e008f71"
    extrinsics: [
      "0x210284008eaf04151687736326c9fea1…87613693c912909cb226aa4794f26a48"
    ]
    pendingExtrinsicsCount: 0
    ump: {}
INFO (block-builder/14701): Try building block #4,286,006
    number: 4286006
    extrinsicsCount: 1
    umpCount: 0
tx events [
  <tx events>
]
INFO (block-builder/14701): Block built
    number: 4286006
    hash: "0xcd8e6b58affca45dae23d1321cf0ff22897d0ae8782c6703bcf31ccba8a0accb"
    extrinsics: [
      "0xf90184008eaf04151687736326c9fea1…01000000000000000bc38c602cde2200"
    ]
    pendingExtrinsicsCount: 0
    ump: {}
```


## More References
- Homa source code can be found [here](https://github.com/AcalaNetwork/Acala/tree/master/modules/homa).
- You can learn more about `@polkadot/api` [here](https://polkadot.js.org/docs/api).
- [chopsticks doc](https://github.com/AcalaNetwork/chopsticks#chopsticks)
