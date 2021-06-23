# Cross Chain DeFi

## Cross-chain DeFi with Acala

Acala is the DeFi shard aka the finance hub on Polkadot's multi-chain universe. It provides a suite of financial primitives including multi-collateralized stablecoin, trustless staking derivative and a decentralized exchange. These primitives are offered via Acala's DApp directly to the end-users, and also as SDKs for more DApps to be built upon. We foresee a more interconnected, autonomous, more sophisticated, cross-blockchain finance ecosystem at the crust of being fostered.

Here we will feature projects collaborating and experimenting with Acala.

### Laminar

Laminar Chain is a specialist high performance synthetic asset and margin trading chain based-on Substrate. It will use Acala Dollar \(ticker: aUSD\) as its base trading currency, to mint synthetic stablecoins such as EUR and JPY. It also introduces a variety of trading instruments including leveraged trading of forex pairs, gold, stocks, and synthetic crypto pairs BTCUSD and ETHUSD. Value transfer \(at this stage aUSD\) will implement [a token standard](https://github.com/w3f/PSPs/pull/3) accepted by the community.

### Ren

Ren is a reputable inter-blockchain asset bridge, currently bringing BTC, BCH and ZEC to fuel Ethereum DeFi use cases. Ren [builds with Acala](https://github.com/AcalaNetwork/Acala/wiki/U.-Build-with-Acala) by deploying its [RenVM bridge module](https://github.com/AcalaNetwork/Acala/tree/master/ecosystem-modules/ren/renvm-bridge) on the Acala network. The BTC gateway is provided by RenVM, while minting and burning of renBTC on Acala is provided by this gateway module. A couple of upfront benefits that Ren users would enjoy are as follows:

1. a brand new account with only freshly minted renBTC, can perform any transactions on Acala without needing a fee token. Thanks to Acala's FlexiFee feature, tokens like renBTC is integrated natively as one of the default fee tokens alongside with ACA, aUSD and DOT.
2. to promote the usage of renBTC on Acala and wider Polkadot ecosystem, fees of minting renBTC can be waived without compromising security

Ren is a good example for DeFi projects considering gaining access to Polkadot's multi-chain ecosystem, and leveraging Acala's customized/optimized finance platform, without upfront economic and technical burdens for building a separate blockchain \(or so called parachain\).

## Mandala Test Network

On Mandala testnet the cross-chain functionality is provided by a trusted services, and will use Polkadot's XCMP \(Cross-chain Message Passing\) mechanism as soon as it becomes available.

### Laminar Chain

![lamianr](https://github.com/AcalaNetwork/Acala/wiki/image/cross-laminar.png)

* [Laminar Flow Exchange](https://flow.laminar.one/)
* [Get Started Guide](https://github.com/laminar-protocol/laminar-chain/wiki/1.-Get-Started)
* [Video: Margin Trading](https://youtu.be/s4pl6glM5kA)

#### Transfer aUSD to Laminar Chain

 Go to `Wallet` --&gt; tab `Cross-chain` --&gt; select `Inter Polkadot Transfer`. Next, select `To Chain` "Laminar", set currency as "aUSD" and put the account you want to transfer to and put the amount of aUSD you would like to send across to Laminar Chain, and click `Transfer`. The aUSD will be send to the same account on Laminar Chain.
 
![transfer](https://i.imgur.com/0hcakPv.png)

#### Transfer aUSD back from Laminar Chain

![transferback](https://github.com/AcalaNetwork/Acala/wiki/image/cross-laminartoacala.png)

On [Laminar Flow Exchange](https://flow.laminar.one/), go to `Dashboard` --&gt; click `transfer` next to aUSD, then simply put the amount of aUSD you would like to send across to Acala, and click `Confirm`. The aUSD will be send to the same account on Laminar Chain.

### Ren

![lamianr](https://github.com/AcalaNetwork/Acala/wiki/image/cross-renbtc.png)

#### Minting renBTC

On Mandala testnet, the faucet drips 0.2 renBTC each time, and capped at 1 renBTC/month/user. Upon canary network and mainnet, RenVM will be used for minting and burning renBTC.

![laminar](https://i.imgur.com/mCAKXyF.png)

#### Using renBTC

The collaboration with Ren will boost the asset classes available on Acala and bring brand new use cases for Bitcoin:

* renBTC can be used as collateral for aUSD credit facility
* renBTC can be traded on Acala DeX for other assets like DOT, LDOT, ACA, aUSD, XBTC
* Users can now take part in Polkadot Proof-of-Stake validation and earn staking returns by trading DOT and - using the staking derivative protocol
* Providing liquidity to DeX and earning additional yields is now a smooth flow
* aUSD can be transferred to the Laminar Chain for margin trading synthetic forex pairs e.g. EURUSD, gold and stocks, and synthetic BTC & ETH.

