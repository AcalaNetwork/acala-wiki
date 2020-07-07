# Oracle Guide for Mandala Test Network

Price feeds on Mandala are rather arbitrary. DOT is set at $300 until a proper market establishes. ACA mirrors MKR price until market bootstrap. Other crypto prices like BTC come from [Alpha Vantage](https://www.alphavantage.co/).

Below are simple guidelines for

1. Check Membership
2. Set Up an Oracle Server
3. Request for Membership

### Check Membership \(via Polkadot-UI\)

![Check](https://github.com/AcalaNetwork/Acala/wiki/image/oracle_membership.png)

### Request for Membership

Membership is by invitation only. If you would like to become an oracle provider, please submit your interest [here](https://forms.gle/NnEqCZK49d1jUT696)

### Set Up an Oracle Server

The default oracle server we are using is open sourced [here](https://github.com/laminar-protocol/oracle-server). Follow the instructions there to set up environment variable, build and start the server.

* `SUB_KEY_SEED`: is the hex raw seed of your oracle server account. Make sure it has enough ACA for transaction fees.
* `SUB_ENDPOINT`: `wss://node-6632097881473671168.au.onfinality.cloud/ws`
* `PRICE_FEED_INTERVAL_MS`: price feed interval in milli seconds e.g. `300000` as 5 minutes
* `FEED_ACALA`: as `true`
* `CONSOLE_LOG`: as `true` for logging

 

![Check](https://github.com/AcalaNetwork/Acala/wiki/image/oracle_successlog.png)

  


![Check](https://github.com/AcalaNetwork/Acala/wiki/image/oracle_tx.png)

