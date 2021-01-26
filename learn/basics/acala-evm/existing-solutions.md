# Existing Solutions

Current Substrate EVM compatibility solution i.e. [Frontier](https://github.com/paritytech/frontier) emulates the Ethereum node. It aims to implement the full set of Ethereum RPC and to emulate the Ethereum block production process. This allows existing Ethereum tools such as Metamask and Remix to work with a Frontier enabled node seamlessly. Comment

Integrating Frontier has revealed the following challenges by their severity:

### **1. Confined inside the EVM Sandbox**

Frontier allows users to interact with EVM via Metamask or other existing Ethereum tools, none of which fully support Substrate yet. Any functionalities from native modules of Substrate, such as Acala DeX, token pallets that support multiple currency and cross-chain capability, and any other pallets built by any parachains such as margin trading pallet by Laminar can not be accessed via Metamask or other existing Ethereum tools.

This means users will need to use a Substrate wallet \(e.g. Polkadot-js Extension\) and Metamask at the same time if they ever want to taste the real power of Acala, Substrate or Polkadot for that matter. This is certainly a deal-breaker for us.

### **2. Making Nodes more Expensive**

[Frontier](https://github.com/paritytech/frontier) is heavy by design. Substrate does not store transactions by hash nor historical events, nor does it provide any event filtering ability. Substrate nodes are lightweight by design in order to minimize resource usage \(disk space & CPU\).

Ethereum nodes, on the other hand, allow users to query transactions by hash and offer powerful event log querying API. Frontier injects special block importing logic, storing the transactions and events into an off-chain auxiliary store in order to power the query API required by Ethereum.

This adds maintenance costs as it requires more powerful machines and larger disk space to operate a node. This goes against the goal to have a lightweight node to lower barriers for people from anywhere to run nodes which helps the network to be more decentralized.

