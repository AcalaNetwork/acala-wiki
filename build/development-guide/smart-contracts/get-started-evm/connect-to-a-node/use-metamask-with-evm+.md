---
description: >-
  Instructions on how to connect MetaMask to Acala EVM+ in order to interact
  with the smart contracts deployed on it.
---

# Use MetaMask with EVM+

{% hint style="info" %}
To connect to a network other than Mandala TC7, replace the values from the detailed instructions with the corresponding values from the bottom of this page.
{% endhint %}

## Mandala TC8

In order to be able to interact with the Acala EVM+ in Mandala TC8, you first need to navigate to the **Add network** section of the MetaMask. You can find it at the bottom of the list of available networks after clicking on the currently active network.

![MetaMask => Currently active network => Add network](<../../../../../.gitbook/assets/Screenshot 2022-03-02 at 02.22.49.png>)

This should open up a form to add a new network to your MetaMask (you might have to unlock MetaMask before it opens). Once the form is opened, use the following information to add the Mandala TC8 network:

<table><thead><tr><th>Key</th><th>Value</th><th data-hidden></th></tr></thead><tbody><tr><td><strong>Network name</strong></td><td>Mandala TC8</td><td></td></tr><tr><td><strong>New RPC URL</strong></td><td><code>https://eth-rpc-mandala.aca-staging.network</code></td><td></td></tr><tr><td><strong>Chain ID</strong></td><td>595</td><td></td></tr><tr><td><strong>Currency symbol</strong></td><td>ACA</td><td></td></tr><tr><td><strong>Block Explorer URL</strong></td><td><code>https://blockscout.mandala.acala.network/</code></td><td></td></tr></tbody></table>

![Mandala TC7 connection details](<../../../../../.gitbook/assets/image (26).png>)

Mandala TC8 should now be connected and you should see your ACA balance (if you already have it).

![MetaMask connected to Mandala TC7](<../../../../../.gitbook/assets/image (75).png>)

{% hint style="info" %}
You might have to bind your MetaMask account to your Substrate account in order to see your balance.
{% endhint %}

## Acala main network

<table data-header-hidden><thead><tr><th>Key</th><th>Value</th><th data-hidden></th></tr></thead><tbody><tr><td><strong>Name</strong></td><td>Acala</td><td></td></tr><tr><td><strong>Primary URL</strong></td><td><code>https://eth-rpc-acala.aca-api.network/</code></td><td></td></tr><tr><td><strong>Alternative URL</strong></td><td><code>https://rpc.evm.acala.network/</code></td><td></td></tr><tr><td><strong>Chan ID</strong></td><td>787</td><td></td></tr><tr><td><strong>Explorer</strong></td><td><code>https://blockscout.acala.network/</code></td><td></td></tr><tr><td><strong>WS endpoint URL</strong></td><td><code>wss://eth-rpc-acala.aca-api.network/ws</code></td><td></td></tr><tr><td><strong>SubQL URL</strong></td><td><code>https://acala-evm-subql.aca-api.network</code></td><td></td></tr><tr><td><strong>Symbol</strong></td><td>ACA</td><td></td></tr></tbody></table>

## Karura main network

<table data-header-hidden><thead><tr><th>Key</th><th>Value</th><th data-hidden></th></tr></thead><tbody><tr><td><strong>Name</strong></td><td>Karura</td><td></td></tr><tr><td><strong>Primary URL</strong></td><td><code>https://eth-rpc-karura.aca-api.network/</code></td><td></td></tr><tr><td><strong>Alternative URL</strong></td><td><code>https://rpc.evm.karura.network/</code></td><td></td></tr><tr><td><strong>Chan ID</strong></td><td>686</td><td></td></tr><tr><td><strong>Explorer</strong></td><td><code>https://blockscout.karura.network/</code></td><td></td></tr><tr><td><strong>WS endpoint URL</strong></td><td><code>wss://eth-rpc-karura.aca-api.network/ws</code></td><td></td></tr><tr><td><strong>SubQL URL</strong></td><td><code>https://karura-evm-subql.aca-api.network</code></td><td></td></tr><tr><td><strong>Symbol</strong></td><td>KAR</td><td></td></tr></tbody></table>
