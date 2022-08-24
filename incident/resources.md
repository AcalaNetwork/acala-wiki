# Resources

### Table of Trace Results:

* [Trace Result #1: 08-15-2022](https://acala.discourse.group/t/08-14-2022-incident-on-chain-trace-results/1134#trace-result-1-08-15-2022-3)
* [Trace Result #2: 08-17-2022](https://acala.discourse.group/t/08-14-2022-incident-on-chain-trace-results/1134#trace-result-2-08-17-2022-6)
* [Trace Result #3: 08-18-2022](https://acala.discourse.group/t/08-14-2022-incident-on-chain-trace-results/1134#trace-result-3-08-18-2022-dot-outflow-9)
* [Trace Result #4: 08-19-2022](https://acala.discourse.group/t/08-14-2022-incident-on-chain-trace-results/1134#trace-result-4-08-19-2022-top-destination-address-12)



### Updates

* [Updates on the aUSD Incident — 22 August 2022](https://medium.com/acalanetwork/updates-on-the-ausd-incident-22-august-2022-997efec98b35)



### Community Frequently Asked Questions:

#### **I have LDOT and LCDOT tokens on Acala. Are these impacted by the aUSD error mint situation and is there any possibility that these funds will leave my possession?**

The trace is address-based. We are tracing addresses that claimed the aUSD error mints, and all subsequent addresses that they interacted with, including the DEX module and all the addresses that interacted with it and so on. Please see the trace reports [here](https://acala.discourse.group/t/08-14-2022-incident-on-chain-trace-results/1134). The trace is still in progress, a full trace report will be published as soon as it’s finished. LCDOT product itself is not impacted, redemption of LCDOT is 1:1 to DOT upon Acala parachain lease completion. The DOTs contributed to the crowdloan will be locked on Polkadot until then. LDOT staking and unstaking (waiting for normal unbonding period) is also not impacted. All impacted services are listed [here](https://wiki.acala.network/misc/acala-dapp-status).



