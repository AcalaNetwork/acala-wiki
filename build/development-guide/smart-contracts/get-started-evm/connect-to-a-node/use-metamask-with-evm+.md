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

| Key                    | Value                                         |   |
| ---------------------- | --------------------------------------------- | - |
| **Network name**       | Mandala TC8                                   |   |
| **New RPC URL**        | `https://eth-rpc-mandala.aca-staging.network` |   |
| **Chain ID**           | 595                                           |   |
| **Currency symbol**    | ACA                                           |   |
| **Block Explorer URL** | `https://blockscout.mandala.acala.network/`   |   |

![Mandala TC7 connection details](<../../../../../.gitbook/assets/image (2) (2).png>)

Mandala TC8 should now be connected and you should see your ACA balance (if you already have it).

![MetaMask connected to Mandala TC7](<../../../../../.gitbook/assets/image (43).png>)

{% hint style="info" %}
You might have to bind your MetaMask account to your Substrate account in order to see your balance.
{% endhint %}

## Acala main network

| **Name**            | Acala                                     |   |
| ------------------- | ----------------------------------------- | - |
| **Primary URL**     | `https://eth-rpc-acala.aca-api.network/`  |   |
| **Alternative URL** | `https://rpc.evm.acala.network/`          |   |
| **Chan ID**         | 787                                       |   |
| **Explorer**        | `https://blockscout.acala.network/`       |   |
| **WS endpoint URL** | `wss://eth-rpc-acala.aca-api.network/ws`  |   |
| **SubQL URL**       | `https://acala-evm-subql.aca-api.network` |   |
| **Symbol**          | ACA                                       |   |

## Karura main network

| **Name**            | Karura                                     |   |
| ------------------- | ------------------------------------------ | - |
| **Primary URL**     | `https://eth-rpc-karura.aca-api.network/`  |   |
| **Alternative URL** | `https://rpc.evm.karura.network/`          |   |
| **Chan ID**         | 686                                        |   |
| **Explorer**        | `https://blockscout.karura.network/`       |   |
| **WS endpoint URL** | `wss://eth-rpc-karura.aca-api.network/ws`  |   |
| **SubQL URL**       | `https://karura-evm-subql.aca-api.network` |   |
| **Symbol**          | KAR                                        |   |
