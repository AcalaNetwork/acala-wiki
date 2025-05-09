# Full Node

## Spec Requirement

Same as the Polkadot full node requirements.

## Latest Release Version

{% embed url="https://github.com/AcalaNetwork/Acala/releases/latest" %}
Shows the latest release version of Acala, Karura & Mandala
{% endembed %}

## Run from Source Code

* Clone the repo: [https://github.com/AcalaNetwork/acala-node](https://github.com/AcalaNetwork/acala-node)
* Checkout tag here: [https://github.com/AcalaNetwork/acala-node/tags](https://github.com/AcalaNetwork/acala-node/tags)
* Install dependencies using instructions from [here](https://github.com/AcalaNetwork/acala-node?tab=readme-ov-file#building)
* Build Acala: `cargo build --release
* Run `./target/release/acala --chain=acala`

## Using Docker

* Image: `acala/acala-node:latest` or `acala/acala-node:[version number]`
* `docker run acala/acala-node:latest --chain=acala`

## Common CLI

* CLI is mostly the same as any Substrate-based chain such as Kusama and Polkadot
* Because there are two node services are running, `--` is used to split the CLI. Arguments before `--` are passed to the parachain full-node service and arguments after `--` is passed to the Relay Chain full-node service.
  * For example `--chain=parachain.json --rpc-port=9944 -- --chain=relaychain.json --rpc-port=9945` means
    * The parachain service is using `parachain.json` as the chain spec and the web socket RPC port is 9944
    *   The Relay Chain service is using `relaychain.json` as the chain spec and the web socket

        RPC port is 9945
* It is recommended to explicitly specify the ports for both services to avoid confusion
  * For example `--listen-addr=/ip4/0.0.0.0/tcp/30333 --listen-addr=/ip4/0.0.0.0/tcp/30334/ws -- --listen-addr=/ip4/0.0.0.0/tcp/30335 --listen-addr=/ip4/0.0.0.0/tcp/30336/ws`
* It is recommended to add `--execution=wasm` for parachain service to avoid syncing issues.
* It is recommended to add `--relay-chain-rpc-url` or `--relay-chain-rpc-urls` for parachain service to avoid fully sync with the relay chain to work, so in general, they will use fewer system resources.

## Example CLI

### Archive PRC Node

```
--base-path=/acala/data
--chain=acala
--name=rpc-1
--pruning=archive
--rpc-external
--rpc-cors=all
--rpc-port=9944
--rpc-max-connections=2000
--relay-chain-rpc-url=wss://polkadot-rpc.publicnode.com
```
