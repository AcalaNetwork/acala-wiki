# Open HRMP Channel

\(Original hackmd file [here](https://hackmd.io/naPxPYPYSXOlK0L7WohVdQ?view)\)

Steps to open HRMP channels:

1. Sender parachain send a init open channel request.

2. Recipient parachain accept request.

The steps above are done via `Xcm::Transact` from the sender's or recipient's part.

### Send init open channel request

#### Generate encoded transact

In PolkadotJS app, switch to the live Rococo network. Go to **Developer -&gt; Javascript** section.

Run the encoding code, note to replace the demo recipient para id `777` with your recipient:

```javascript
const tx = api.tx.hrmp.hrmpInitOpenChannel(777, 8, 1024);
console.log(tx.toHex())
```

The result will be like `0x3c041600090300000800000000040000`, remove the leading hex `3c04`, and the encoded result is `0x1600090300000800000000040000`.

#### Send request

Go to PolkadotJS app, switch to sender parachain. Go to **Developer -&gt; Sudo** section.

Use `xcmHandler -> sudoSendXcm` to send the transaction.

To confirm the request was sent, switch to live Rococo, go to **Developer -&gt; Chain State**, check **hrmp -&gt; hrmpOpenChannelRequests**.

### Accept channel request

#### Generate encoded transact

In PolkadotJS app, switch to the live Rococo network. Go to **Developer -&gt; Javascript** section.

Run the encoding code, note to replace the demo recipient para id `666` with your recipient:

```javascript
const tx = api.tx.hrmp.hrmpAcceptOpenChannel(666);
console.log(tx.toHex())
```

The result will be like `0x1c0416019a020000`, remove the leading hex `1c04`, and the encoded result is `0x16019a020000`.

#### Send request

Go to PolkadotJS app, switch to recipient parachain. Go to **Developer -&gt; Sudo** section.

Use `xcmHandler -> sudoSendXcm` to send the transaction.

