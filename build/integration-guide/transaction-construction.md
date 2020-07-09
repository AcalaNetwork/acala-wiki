---
description: >-
  This page will discuss the transaction format in Acala and how to create,
  sign, and broadcast transactions.
---

# Transaction Construction

This page will discuss the transaction format in Polkadot and how to create, sign, and broadcast transactions. Like the other pages in this guide, this page demonstrates some of the available tools. **Always refer to each tool's documentation when integrating.**

### Transaction Format

Polkadot has some basic transaction information that is common to all transactions.

* Address: The SS58-encoded address of the sending account.
* Block Hash: The hash of the checkpoint block.
* Block Number: The number of the checkpoint block.
* Genesis Hash: The genesis hash of the chain.
* Metadata: The SCALE-encoded metadata for the runtime when submitted.
* Nonce: The nonce for this transaction.\*
* Spec Version: The current spec version for the runtime.
* Transaction Version: The current version for transaction format.
* Tip: Optional, the tip to increase transaction priority.
* Era Period: Optional, the number of blocks after the checkpoint for which a transaction is valid. If zero, the transaction is immortal.

\*The nonce queried from the System module does not account for pending transactions. You must track and increment the nonce manually if you want to submit multiple valid transactions at the same time.

Each transaction will have its own \(or no\) parameters to add. For example, the `transferKeepAlive` function from the Balances pallet will take:

* `dest`: Destination address
* `#[compact] value`: Number of tokens \(compact encoding\)

Once you have all the necessary information, you will need to:

1. Construct an unsigned transaction.
2. Create a signing payload.
3. Sign the payload.
4. Serialize the signed payload into a transaction.
5. Submit the serialized transaction.

Parity provides the following tools to help perform these steps.

### Acala JS

\[TODO\]

### Tx Wrapper

If you do not want to use the CLI for signing operations, Parity provides an SDK called [TxWrapper](https://github.com/paritytech/txwrapper) to generate and sign transactions offline. See the [examples](https://github.com/paritytech/txwrapper/tree/master/examples) for a guide.

**Import a private key**

```text
import { importPrivateKey } from '@substrate/txwrapper';

const keypair = importPrivateKey(“pulp gaze fuel ... mercy inherit equal”);
```

**Derive an address from a public key**

```text
import { deriveAddress } from '@substrate/txwrapper';

// Public key, can be either hex string, or Uint8Array
const publicKey = “0x2ca17d26ca376087dc30ed52deb74bf0f64aca96fe78b05ec3e720a72adb1235”;
const address = deriveAddress(publicKey);
```

**Construct a transaction offline**

```text
import { methods } from "@substrate/txwrapper";

const unsigned = methods.balances.transferKeepAlive(
  {
    dest: "15vrtLsCQFG3qRYUcaEeeEih4JwepocNJHkpsrqojqnZPc2y",
    value: 500000000000,
  },
  {
    address: "121X5bEgTZcGQx5NZjwuTjqqKoiG8B2wEAvrUFjuw24ZGZf2",
    blockHash: "0x1fc7493f3c1e9ac758a183839906475f8363aafb1b1d3e910fe16fab4ae1b582",
    blockNumber: 4302222,
    genesisHash: "0xe3777fa922cafbff200cadeaea1a76bd7898ad5b89f7848999058b50e715f636",
    metadataRpc, // must import from client RPC call state_getMetadata
    nonce: 2,
    specVersion: 1019,
    tip: 0,
    eraPeriod: 64, // number of blocks from checkpoint that transaction is valid
    transactionVersion: 1,
  },
  {
    metadataRpc,
    registry, // Type registry
  }
);
```

**Construct a signing payload**

```text
import { methods, createSigningPayload } from '@substrate/txwrapper';

// See "Construct a transaction offline" for "{...}"
const unsigned = methods.balances.transferKeepAlive({...}, {...}, {...});
const signingPayload = createSigningPayload(unsigned, { registry });
```

**Serialize a signed transaction**

```text
import { createSignedTx } from "@substrate/txwrapper";

// Example code, replace `signWithAlice` with actual remote signer.
// An example is given here:
// https://github.com/paritytech/txwrapper/blob/630c38d/examples/index.ts#L50-L68
const signature = await signWithAlice(signingPayload);
const signedTx = createSignedTx(unsigned, signature, { metadataRpc, registry });
```

**Decode payload types**

You may want to decode payloads to verify their contents prior to submission.

```text
import { decode } from "@substrate/txwrapper";

// Decode an unsigned tx
const txInfo = decode(unsigned, { metadataRpc, registry });

// Decode a signing payload
const txInfo = decode(signingPayload, { metadataRpc, registry });

// Decode a signed tx
const txInfo = decode(signedTx, { metadataRpc, registry });
```

**Check a transaction's hash**

```text
import { getTxHash } from ‘@substrate/txwrapper’;
const txHash = getTxHash(signedTx);
```

### Submitting a Signed Payload

There are several ways to submit a signed payload:

1. Signer CLI \(`yarn run:signer submit --tx <signed-transaction> --ws <endpoint>`\)
2. [Substrate API Sidecar](https://wiki.polkadot.network/docs/en/build-node-interaction#substrate-api-sidecar)
3. [RPC](https://wiki.polkadot.network/docs/en/build-node-interaction#polkadot-rpc) with `author_submitExtrinsic` or `author_submitAndWatchExtrinsic`, the latter of which will subscribe you to events to be notified as a transaction gets validated and included in the chain.

### Notes

Some addresses to use in the examples. See [Subkey documentation](https://substrate.dev/docs/en/knowledgebase/integrate/subkey).

```text
$ subkey --network polkadot generate
Secret phrase `pulp gaze fuel ... mercy inherit equal` is account:
  Secret seed:      0x57450b3e09ba4598 ... ... ... ... ... ... ... .. 219756eeba80bb16
  Public key (hex): 0x2ca17d26ca376087dc30ed52deb74bf0f64aca96fe78b05ec3e720a72adb1235
  Account ID:       0x2ca17d26ca376087dc30ed52deb74bf0f64aca96fe78b05ec3e720a72adb1235
  SS58 Address:     121X5bEgTZcGQx5NZjwuTjqqKoiG8B2wEAvrUFjuw24ZGZf2

$ subkey --network polkadot generate
Secret phrase `exercise auction soft ... obey control easily` is account:
  Secret seed:      0x5f4bbb9fbb69261a ... ... ... ... ... ... ... .. 4691ed7d1130fbbd
  Public key (hex): 0xda04de6cd781c98acf0693dfb97c11011938ad22fcc476ed0089ac5aec3fe243
  Account ID:       0xda04de6cd781c98acf0693dfb97c11011938ad22fcc476ed0089ac5aec3fe243
  SS58 Address:     15vrtLsCQFG3qRYUcaEeeEih4JwepocNJHkpsrqojqnZPc2y
```

