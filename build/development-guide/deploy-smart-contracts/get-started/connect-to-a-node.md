# Connect to a Node

To use Acala EVM, you need to connect to an Acala Node. You can either

1. run a local Acala test node
2. connect to a deployed test network \(maintained by Acala\)

### **1. Run a local Acala test node Comment**

To run your own node, you need to have installed [Docker](https://www.docker.com/) on your machine. If you don’t have it installed, please follow the instructions [here](https://docs.docker.com/get-docker/).

To check whether Docker is successfully installed run the command below:

```text
docker version
```

If you receive the version number, you can start your local Acala node with the command below:

```text
docker pull acala/acala-node:latest
docker run -it -p 9944:9944 -p 9933:9933 acala/acala-node:latest --dev --ws-external --rpc-external --rpc-cors=all
```

The output of your node should look like this:

![](https://i.imgur.com/EyryyFs.png)

### **2. Connect to a deployed test network**

Find all available testnet nodes [here](https://wiki.acala.network/learn/get-started/public-nodes#mandala-test-network-nodes)

