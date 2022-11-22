# Connect to a Node

To use Acala EVM, you need to connect to an Acala Node. You can either

1. connect to a deployed test network (maintained by Acala) OR
2. run a local Acala test node

## **1. Connect to a deployed test network**

Find all available testnet nodes [here](../../../../../get-started/networks.md#rpc-endpoints). You can use [Polkadot Explorer](../acala-console.md) to communicate with the node, and [EVM Playground](../evm-playground.md) to deploy and execute contracts.

## **2. Run a local Acala test node**

Alternatively, you can run a local test node. To run your own node, you need to have installed [Docker](https://www.docker.com/) on your machine. If you don’t have it installed, please follow the instructions [here](https://docs.docker.com/get-docker/).

To check whether Docker is successfully installed run the command below:

```bash
docker version
```

If you receive the version number, you can start your local Acala node with the command below:

```bash
docker pull acala/acala-node:latest
docker run -it -p 9944:9944 -p 9933:9933 acala/acala-node:latest --dev --ws-external --rpc-external --rpc-cors=all
```

The output of your node should look like this:

![](https://i.imgur.com/EyryyFs.png)
