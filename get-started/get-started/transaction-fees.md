# Transaction Fees

**🔔 Karura allows fees to be paid in any supported token.** However once transfer is enabled, there will be a period of time transaction fees are required to be paid in the native token $KAR, until KSM/KAR, kUSD/KAR and other [Karura Swap](https://apps.karura.network/swap) pools are bootstrapped, then KSM, kUSD and other tokens can be used to pay transaction fees.

Transaction fees are used to prevent users from consuming too much limited resources of the blockchain, such as storage and computation power. Karura uses weight-based fees, unlike gas, are predictable and charged pre-dispatch.&#x20;

### Fee Estimates

These are rough estimates of typical relevant transactions:

* **Transfer from Kusama to Karura,** there are two components to the cross-chain transfer fees:
  * Kusama fee, determined by Kusama
  * Karura fee: 0.3 milliKSM\~
* **Transfer from Karura to Kusama,** there are two components to the cross-chain transfer fees:
  * Karura fee: 4 milliKAR (0.004 KAR)\~
  * Kusama fee, determined by Kusama
* **Karura**&#x20;
  * **Transfer:** 4 milliKAR (0.004 KAR)\~
  * **DeX swap:** 12 milliKAR\~
  * **Add liquidity:** 18 milliKAR\~
  * **Adjust kUSD loan:** 18 milliKAR\~

Note: these are estimates and will be changed based on actual transaction size, network conditions and other factors.

For now, all transaction fees go to the Karura Treasury - a tiny contribution to a sustainable future. Read more [here](../../learn/treasury.md).

### Fee Adjustment <a href="#fee-adjustment" id="fee-adjustment"></a>

Fees on Karura are adjusted based on transaction volume, while still predictable. Karura has a block fullness target, fees increase or decrease for the next block based on the fullness of the current block relative to the target.&#x20;

### Bring Your Own Gas

Why users are restricted to pay fees in the native token when transferring other tokens?! On Karura you don't need to. Users can pay fees in any tokens that are supported on the Karura network. When paying fees in tokens other than KAR, fees are still estimated in KAR, a real-time swap operation (between paid token and KAR) is executed automatically by the chain. This operation is atomic and transparent to the users.&#x20;
