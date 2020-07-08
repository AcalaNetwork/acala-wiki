---
description: Here is a guide to set up a node and ways to participate in the Acala.
---

# Mandala Test Network Maintainers Guide

### Full Node

Mandala Test Network is a risk-free and value-free playground, purely for testing out functionalities and "explosive" experiments. There is no network value nor rewards. However you are welcome to run a node and join the network, to try it out, prepare for Karura Network \(that will join Kusama with network value\) and Acala Mainnet, or just for the love of it.

#### Run a Full Node \(Mandala\)

**Using Docker**

Install docker with Linux:  
`wget -qO- https://get.docker.com/ | sh`

If you have docker installed, you can use it to start your node without needing to build from the code. Here is the command

```text
docker run -d --restart=always -p 30333:30333 -p 9933:9933 -p 9944:9944 -v node-data:/acala/data acala/acala-node:latest --chain mandala --base-path=/acala/data/01-001 --ws-port 9944 --rpc-port 9933 --port 30333 --ws-external --rpc-external --ws-max-connections 1000 --rpc-cors=all --unsafe-ws-external --unsafe-rpc-external --pruning=archive --name "Name of Telemetry" 
```

