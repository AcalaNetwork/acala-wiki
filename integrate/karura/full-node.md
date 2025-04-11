# Full Node

## Spec Requirement

You can check if your machine satisfy the spec requirement by using the following make command to benchmark your machine.

```
// If you are running docker image:
docker run acala/acala-node:latest benchmark machine --chain=karura

// If you are using dev environment:
make benchmark-machine
```

Note:

You need to setup your dev environment first for make commands to work.

The benchmark result will look similar to this: ![](../../networks/home/integration/machine-benchmark.png)

## Run from Source Code

* Clone the repo: [https://github.com/AcalaNetwork/acala-node](https://github.com/AcalaNetwork/acala-node)
* Checkout tag here: [https://github.com/AcalaNetwork/acala-node/tags](https://github.com/AcalaNetwork/acala-node/tags)
  Install dependencies using instructions from [here](https://github.com/AcalaNetwork/acala-node?tab=readme-ov-file#building)
* Build Karura: `cargo build --release`
* Run `./target/release/acala --chain=karura`

## Using Docker

* Image: `acala/acala-node:latest` or `acala/acala-node:[version number]`
* `docker run acala/acala-node:latest --chain=karura`

## Common CLI

* CLI is mostly the same as any Substrate-based chain such as Polkadot and Kusama
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
--chain=karura
--name=rpc-1
--pruning=archive
--rpc-external
--rpc-cors=all
--rpc-port=9944
--rpc-max-connections=2000
--relay-chain-rpc-url=wss://kusama-rpc.publicnode.com
```
