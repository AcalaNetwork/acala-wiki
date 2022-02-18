# Open HRMP Channel

(Original hackmd file [here](https://hackmd.io/naPxPYPYSXOlK0L7WohVdQ?view))

Steps to open HRMP channels:

1. Sender parachain send a init open channel request.
2. Recipient parachain accept request.

The steps above are done via `Xcm::Transact` from the sender's or recipient's part.

Given example of Karura and Statemine bi-direction channel communicate, there're two steps needs to be done on each parachain side:

Karura:

* `hrmpInitOpenChannel(1000)` : Initiate open HRMP channel request to Statemine
* `hrmpAcceptOpenChannel(1000)` : Accept HRMP channel request from Statemine

Statemine:

* `hrmpInitOpenChannel(2000)` : Initiate open HRMP channel request to Karura
* `hrmpAcceptOpenChannel(2000)` : Accept HRMP channel request from Karura

If Karura and Statemine just have one single direction channel which only allow Karura to Statemine but not allow Statemine to Karura, then there're only one steps done on each side:

* Karura: `hrmpInitOpenChannel(1000)`: Initiate open HRMP channel request to Statemine
* Statemine: `hrmpAcceptOpenChannel(2000)` : Accept HRMP channel request from Karura

In the example below, we're doing Karura to Statemine single one direction open channel.

## Send init open channel request

### Generate encoded transact

In PolkadotJS app, switch to the live Polkadot/Karura network. Go to **Developer -> Javascript** section. Run the encoding code, note to replace the demo recipient para id `1000` with your recipient:

```javascript
const tx = api.tx.hrmp.hrmpInitOpenChannel(1000, 1000, 102400);
console.log(tx.toHex());
```

The result will be like `0x3c043c00e8030000e803000000900100`, remove the leading hex `3c04`, and the encoded result is `0x3c00e8030000e803000000900100`.

### Send request

Go to PolkadotJS app, switch to sender parachain(here is Karura). Go to **Developer -> Sudo** section.

Use `ormlXcm -> sendAsSovereign` to send the transaction. The xcm message format(v2):

Notice there're two field should be changed:&#x20;

* \<relay-chain-encoded-hex-call>: `0x3c00e8030000e803000000900100`
* \<parachain-sovereign-account>: 5Ec4AhPUwPeyTFyuhGuBbD224mY85LKLMSqSSo33JYWCazU4

> Note: the sovereign account of parachain 2000 is: 5Ec4AhPUwPeyTFyuhGuBbD224mY85LKLMSqSSo33JYWCazU4. in your case, if your parachain intergrated `orml-xcm`, then you should change the \<parachain-sovereign-account> to your own parachain sovereign account.

```
ormlXcm.sendAsSovereign(
  dest: XcmVersionedMultiLocation
  {
    V1: {
      parents: 1
      interior: Here
    }
  }
  
  message: XcmVersionedXcm
  {
    V2: [
      {
        WithdrawAsset: [
          {
            id: {
              Concrete: {
                parents: 0
                interior: Here
              }
            }
            fun: {
              Fungible: 1,000,000,000,000
            }
          }
        ]
      }
      {
        BuyExecution: {
          fees: {
            id: {
              Concrete: {
                parents: 0
                interior: Here
              }
            }
            fun: {
              Fungible: 40,000,000,000
            }
          }
          weightLimit: Unlimited
        }
      }
      {
        Transact: {
          originType: Native
          requireWeightAtMost: 1,000,000,000
          call: {
            encoded: <relay-chain-encoded-hex-call>
          }
        }
      }
      {
        DepositAsset: {
          assets: {
            Wild: All
          }
          maxAssets: 1
          beneficiary: {
            parents: 0
            interior: {
              X1: {
                AccountId32: {
                  network: Any
                  id: <parachain-sovereign-account>
                }
              }
            }
          }
        }
      }
    ]
  }
)
```

To confirm the request was sent, switch to Polkadot/Kusama, go to **Developer -> Chain State**, check **hrmp -> hrmpOpenChannelRequests**.

## Accept open channel request

### Generate encoded transact

In PolkadotJS app, switch to the live Polkadot/Karura network. Go to **Developer -> Javascript** section. Run the encoding code, note to replace the demo recipient para id `2000` with your recipient:

```javascript
const tx = api.tx.hrmp.hrmpAcceptOpenChannel(2000);
console.log(tx.toHex());
```

The result will be like `0x1c043c01d0070000`, remove the leading hex `1c04`, and the encoded result is `0x3c01d0070000`.

### Send request

Go to PolkadotJS app, switch to recipient parachain. Go to **Developer -> Sudo** section.

Use `polkadotXcm -> send` to send the transaction. The xcm message format likes:

Notice there're one field should be changed:&#x20;

* \<relay-chain-encoded-hex-call>: `0x3c01d0070000`

```
polkadotXcm.send(
  dest: XcmVersionedMultiLocation
  {
    V1: {
      parents: 1
      interior: Here
    }
  }
  
  message: XcmVersionedXcm
  {
    V2: [
      {
        WithdrawAsset: [
          {
            id: {
              Concrete: {
                parents: 0
                interior: Here
              }
            }
            fun: {
              Fungible: 1,000,000,000,000
            }
          }
        ]
      }
      {
        BuyExecution: {
          fees: {
            id: {
              Concrete: {
                parents: 0
                interior: Here
              }
            }
            fun: {
              Fungible: 1,000,000,000,000
            }
          }
          weightLimit: Unlimited
        }
      }
      {
        Transact: {
          originType: Native
          requireWeightAtMost: 1,000,000,000
          call: {
            encoded: <relay-chain-encoded-hex-call>
          }
        }
      }
    ]
  }
)
```
