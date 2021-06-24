# Integration

## Tokens

* **Token decimals:**
  * Karura \(KAR\): 12
  * LKSM: 12
  * Karura Dollar \(kUSD\): 12
* **Base unit:** “Plank"
* **Balance type:**
* **Total Fixed Supply of KAR:** 100,000,000

## Account

### Address Format

Karura uses the [SS58 \(Substrate\) address format](https://github.com/paritytech/substrate/wiki/External-Address-Format-%28SS58%29). Relevant SS58 prefixes are:

* **Acala**: 10 \([ss58 registry details](https://github.com/paritytech/substrate/blob/df4a58833a650cf37fc97764bf6c9314435e3cb2/ss58-registry.json#L103-L111)\)
* **Karura**: 8 \([ss58 registry details](https://github.com/paritytech/substrate/blob/df4a58833a650cf37fc97764bf6c9314435e3cb2/ss58-registry.json#L85-L92)\)
* **Mandala**: 

### Existential Deposit

Karura uses an _existential deposit_ \(ED\) to prevent dust accounts from bloating state. If an account drops below the ED, it will be removed from storage and the nonce reset. 

Kaura's ED is TBD.

## Karura PRC Endpoints

* `wss://karura.api.onfinality.io/public-ws`
* `wss://karura-rpc-0.aca-api.network`
* `wss://karura-rpc-1.aca-api.network`
* `wss://karura-rpc-2.aca-api.network/ws`
* `wss://karura-rpc-3.aca-api.network/ws`

## Types

Type buddle [here](https://unpkg.com/browse/@acala-network/type-definitions@0.7.4-19/json/typesBundle.json).

