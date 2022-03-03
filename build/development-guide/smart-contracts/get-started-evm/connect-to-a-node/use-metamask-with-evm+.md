---
description: >-
  Instructions on how to connect MetaMask to Acala EVM+ in order to interact
  with the smart contracts deployed on it.
---

# Use MetaMask with EVM+

{% hint style="info" %}
Since EVM is currently only available in Mandala TC7, the page will only cover the connection instructions to it. As we release the EVM+ to the main network, this page will be updated with the required details.
{% endhint %}

## Mandala TC7

In order to be able to interact with the Acala EVM+ in Mandala TC7, you first need to navigate to the **Add network** section of the MetaMask. You can find it at the bottom of the list of available networks after clicking on the currently active network.

![MetaMask => Currently active network => Add network](<../../../../../.gitbook/assets/Screenshot 2022-03-02 at 02.22.49.png>)

This should open up a form to add a new network to your MetaMask (you might have to unlock MetaMask before it opens). Once the form is opened, use the following information to add the Mandala TC7 network:

| Key                    | Value                           |   |
| ---------------------- | ------------------------------- | - |
| **Network name**       | Mandala TC7                     |   |
| **New RPC URL**        | https://tc7-eth.aca-dev.network |   |
| **Chain ID**           | 595                             |   |
| **Currency symbol**    | ACA                             |   |
| **Block Explorer URL** | /                               |   |

![Mandala TC7 connection details](<../../../../../.gitbook/assets/image (40).png>)

Mandala TC7 should now be connected and you should see your ACA balance (if you already have it).

![MetaMask connected to Mandala TC7](<../../../../../.gitbook/assets/image (43).png>)

{% hint style="info" %}
You might have to bind your MetaMask account to your Substrate account in order to see your balance.
{% endhint %}
