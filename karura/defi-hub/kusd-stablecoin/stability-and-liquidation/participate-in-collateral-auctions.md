# Participate in Collateral Auctions

Anyone can participate in collateral auctions and bid for the collaterals in kUSD.

## The Process for Bidders

* You can monitor on-chan events to know when there is an auction
* You can place a bid by sending kUSD to a specific auction
* Once your bid is accepted, your kUSD bid will be transferred from your address to the auction
* If your bid is outbidded by the next bider, then losing bid is refunded back to your address

### Parameters 

These are important parameters for the auction, and they are under the jurisdiction of governance. 

* You bid must be at least the `last bid + minimum price increment` 
* The auction will run for the duration of the `AuctionTimeToClose` if no new bids is placed. However if there is a new bid placed, the auction will be extended for this same period of time.
* `AuctionDurationSoftCap` if an auction passes this period, it will be expedited where `price increment` will be doubled, and `auction time` will be halved. 

Read more on the auction process and parameters [here](liquidation.md#liquidate-on-collateral-auction).

## Monitor Events

* `LiquidateUnsafeCDP`
* `NewCollateralAuction`
* `CollateralAuctionDealt` 

## Place a Bid

You can place a bid by submitting the `auciton id` and the `value` you want to bid for. 

### Extrinsics

This is the Extrinsics \(transaction\) for placing a bid.

```text
auction.bid(id, value)
```



