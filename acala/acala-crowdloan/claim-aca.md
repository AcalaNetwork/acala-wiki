# Claim ACA

**Most crowdloan contributors do not need to claim their ACA or lcDOT tokens. They will be automatically distributed to you.** However, if you participated in the Acala crowdloan through the https://polkadot.js.org/apps/ website, you will need to read and agree to our Terms and Conditions in order to claim your tokens.

If you are unsure, you can also check to see if you need to go through the claims process by going to our [distribution website](https://distribution.acala.network). Once you enter your address, if your status is `To Be Claimed`, then you need to go through the claims process.

## Option 1: Using Polkadot{js} Browser Extension

Navigate to the [claims website](https://distribution.acala.network/claim/acala). Connect your Polkadot{js} Extension, **using the same account that participated in the crowdloan event**, and follow the prompts to complete the process.

You’ll be required to use the extension to sign a message, but it does not cost any transaction fees. Once the process is completed, it may take up to 48 hours for distribution to be scheduled.

## Option 2: Using System Remark

If you are unable to use Sign and verify to sign the message e.g. you used a proxy account to participate or the agency (e.g. wallet) you used to participate in the crowdloan does not have a sign and verify facility, then you can send a System Remark on the Polkadot chain to claim ACA.

1\) Log onto the [Polkadot.js Apps - Polkadot](https://polkadot.js.org/apps/#/explorer), you must be connected to the Polkadot network.

2\) Go to the Developer-Extrinsics section.

![](https://lh4.googleusercontent.com/IJeL--Hr5Zrvho69q2fDJrEu4bQuvQv-VlW4oOUVGQD3dTsmZ0sSFy8nwWnwfofbPH-v\_88pn4COmz4Lg-rDCOlZG8WUa-9FYqSabu\_9Owbn-FOgwtACSQpgRTyUb9NpJMm-vgT-)

3\) **You must select the same account that was used in the Acala crowdloan.** In the submit the following extrinsic field, select system then remarkWithEvent(\_remark) in the drop-down menu.

![](https://lh4.googleusercontent.com/-z9En1Y8GK4ZCjlt3V5ABDl4EJAhd3lMihjsFUr8SD4lzrUR9qaJOf3p\_xSsd6TZlWKATVPxsaI5UYFmzKusWFW1EgDhycN4b8-F\_C3OcsogRoMMbpqTg3jPflSuXdeQJsRHXSj6)

4\) In the \_remark: Bytes field, enter the message required to sign. Copy and paste in the required message to sign (shown on the Claim website).

`I hereby agree to the terms of the statement whose SHA-256 multihash is QmeUtSuuMBAKzcfLJB2SnMfQoeifYagyWrrNhucRX1vjA8. (This may be found at the URL:` [`https://acala.network/acala/terms`](https://acala.network/acala/terms)`)`

Click `Submit Transaction`. Note that you’ll be required to pay a small fee to initiate the transaction so make sure you have some funds in your account.

![](https://lh6.googleusercontent.com/tIGP5ZHl8r\_XNT5EKKvylMLWTitl0IAgFoI0IhhNd58WoNzO0m55xcSSd9HCWSiQccHmsLz4ges17a9qC9SgM3diKGgmRw5eno0y271XOvB5lTDy4sF8HXJtYrA9vi5sCZuqg6eh)

5\) Your remark transaction has been submitted onto Polkadot. You can view the signed remark on [Polkadot Subscan Explorer](https://polkadot.subscan.io). Paste in the Polkadot address used for sending the transaction.

![](https://lh5.googleusercontent.com/17GsCIeXA\_0ijuwCxjuVu3jjRHiSrYaaLMlVY53YkqRpxi6yzTSDP7ASC3BvbAu9ZyWdcxRIQ945fyv0KQK\_aazJ76eZvLqb3\_88hGw6vkL0i\_Ade82Vx100V6TgbewNojuOnKkU)

6\) You’ll see the system(remark\_with\_event) in your transaction history. Click on the corresponding Extrinsic ID.

7\) Copy the `Extrinsic Hash`.

![](https://lh3.googleusercontent.com/w5pF0sy83ZpZ2-CQ\_zO742RD0pqmmSmOI\_1ab1u5ppaCQ05MQSzXVbiHTjsGy3rO1S5TjJE-L7im6dI\_t9qYPy2al9YOh68MZB5cFTSh5xFj3lJR92wA5n0L0LalPXl0Oxca2nH0)

8\) Paste the Extrinsic hash back to the Claim website to complete the process.

Once the process is completed, it may take up to 48 hours for distribution to be scheduled.
