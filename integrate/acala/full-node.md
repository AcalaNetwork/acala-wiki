# Full Node

## Spec Requirement

Same as the Polkadot full node requirements.

## Latest Release Version

{% embed url="https://github.com/AcalaNetwork/Acala/releases/latest" %}
Shows the latest release version of Acala, Karura & Mandala
{% endembed %}

## Run from Source Code

* Clone the repo: [https://github.com/AcalaNetwork/Acala](https://github.com/AcalaNetwork/Acala)
* Checkout tag here: [https://github.com/AcalaNetwork/Acala/tags](https://github.com/AcalaNetwork/Acala/tags)
* Install dependencies using instructions from [here](https://github.com/AcalaNetwork/Acala?tab=readme-ov-file#3-building)
* Build Acala: `cargo build --release --features with-acala-runtime`
* Run `./target/release acala --chain=acala`

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
--
--chain=polkadot
```
