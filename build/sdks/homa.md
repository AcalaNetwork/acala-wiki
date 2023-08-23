# Acala Liquid Staking with Homa

To interact with Acala or Karura from Javascript you can use `@polkadot/api` along with `@acala-network/api`. 

For those looking to stake DOT or unstake LDOT, we also offer the [Homa SDK](https://github.com/AcalaNetwork/acala.js/tree/master/packages/sdk-homa).


## Homa Example

The following example demonstrates how to utilize the Homa SDK and the Polkadot API for staking DOT and unstaking LDOT. 

You can fork the [live example](https://replit.com/@xlc/acalajs-examples#index.js) on replit and run the example yourself.

{% hint style="warning" %}
To run the example on replit, you neeed to first fork it. If you run it directly without forking, it will show an empty webview page. Actual result will be logged to console instead of the webview page, only after you fork it.
{% endhint %}

```js
const { fetchConfig, setupWithServer } = require('@acala-network/chopsticks')
const { ApiPromise, WsProvider } = require('@polkadot/api')
const { Homa, Wallet } = require('@acala-network/sdk')
const { createTestKeyring } = require('@polkadot/keyring')

const main = async () => {
  // use Chopsticks to fork mainnet to a local testnet
  const acalaConfig = await fetchConfig('acala')
  acalaConfig.port = 8111 // set the port
  acalaConfig.block = 4286000
  acalaConfig.db = './db.sqlite'
  const { chain, listenPort, close } = await setupWithServer(acalaConfig)

  // This server can be accessed at wss://acalajs-examples--xlc.repl.co/
  // Use https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Facalajs-examples--xlc.repl.co to connect to this testnet
  console.log('Chopsticks is running on port', listenPort)

  // setup Acala api connect to the local testnet
  const provider = new WsProvider(`ws://localhost:${listenPort}`)
  const api = new ApiPromise({ provider, noInitWarn: true })
  await api.isReady

  // helpers

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

  // ----- query LDOT balance and calculate the DOT amount -----

  // treasury address
  const address = '23M5ttkmR6KcoTAAE6gcmibnKFtVaTP5yxnY8HF1BmrJ2A1i'

  console.log('Address', address)

  const ldot = { Token: 'LDOT' }

  // check LDOT balance
  const balance = await api.query.tokens.accounts(address, ldot)
  const free = balance.free.toNumber()

  console.log('LDOT Balance:', free / 10 ** 10)

  // setup wallet sdk
  const wallet = new Wallet(api)
  await wallet.isReady

  // setup Homa sdk, the liquid staking protocol
  const homa = new Homa(api, wallet)
  await homa.isReady

  // get exchange rate
  const exchangeRate = (await homa.getEnv()).exchangeRate.toNumber()
  console.log('Exchange Rate:', exchangeRate)

  // calculate DOT amount
  const dotAmount = free * exchangeRate
  console.log('DOT amount:', dotAmount / 10 ** 10)

  // ----- Stake LDOT --------

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

  // ----- unstake LDOT ------

  // There aer number of ways to unstake LDOT / convert it back to DOT
  // 1. Slow unstake by wait for 28 days. 
  // 2. Use fast redeem by matching with new stakers. A fast redeem fee is charged. This is only available if there are DOT in the pending pool.
  // 3. Use the Taiga tDOT pool to swap LDOT to DOT. A swap fee is charged. The swap rate depends on the pool balance.
  
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

  // use Taiga tDOT pool to swap

  {
    // use aggregatedDex to perform swap, which allow multiple step swap with both Taiga pool and Acala Dex pool in a single tx
    const tx = api.tx.aggregatedDex.swapWithExactSupply(
      [{Taiga: [/*pool id*/ 0, /*supply asset*/1, /*target asset*/0]}],
      unstakeAmount,
      0 // should always set a min target to prevent unexpected result
    )
    fakeSign(tx, testaddr, 3)

    console.log('Send tx to swap', unstakeAmount / 1e10, 'LDOT')
    const events = await sendTxAndWait(tx)
    console.log('tx events', events.map(x => x.event.toHuman()))
  }
                               
  // shutdown the Chopsticks server
  // await close()
}

main()
```


## More References
- Homa source code can be found [here](https://github.com/AcalaNetwork/Acala/tree/master/modules/homa).
- You can learn more about `@polkadot/api` [here](https://polkadot.js.org/docs/api).
