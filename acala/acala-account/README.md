# Acala Account

This document covers the basics of Acala, Karura, Polkadot and Kusama account addresses.

## Address Format

Acala and Karura use the Substrate-based chain address format SS58. Read more [here](https://wiki.polkadot.network/docs/en/learn-accounts).

* Acala addresses always start with the number 2.
* Karura addresses could start with any letter.

## Account Generation 

Before the network is live, and during the initial launch period, below is the only recommended method for generating an Acala and Karura account:

* Polkadot{.js} Browser Plugin 

## Polkadot{.js} Browser Plugin 

### Install the Browser Plugin

The browser plugin is available for both [Google Chrome](https://chrome.google.com/webstore/detail/polkadot%7Bjs%7D-extension/mopnmbcafieddcagagdcbnhejhlodfdd?hl=en) \(and Chromium based browsers like Brave\) and [FireFox](https://addons.mozilla.org/en-US/firefox/addon/polkadot-js-extension). Download the plugins [here](https://polkadot.js.org/extension/).

![](../../.gitbook/assets/screen-shot-2021-05-14-at-4.49.27-pm.png)

### Create Account

Open the Polkadot{.js} browser extension by clicking the logo on the top bar of your browser. You will see a browser popup not unlike the one below

![](../../.gitbook/assets/screen-shot-2021-05-14-at-4.52.43-pm.png)

Click the big plus button or select "Create new account" from the small plus icon in the top right. The Polkadot{.js} plugin will then use system randomness to make a new seed for you and display it to you in the form of twelve words.

![](../../.gitbook/assets/screen-shot-2021-05-14-at-4.53.46-pm.png)

You should back up these words as [explained here](https://wiki.polkadot.network/docs/en/learn-account-generation#storing-your-key-safely). It is imperative to store the seed somewhere safe, secret, and secure. If you cannot access your account via Polkadot{.js} for some reason, you can re-enter your seed through the "Add account menu" by selecting "Import account from pre-existing seed".

### Name Account & Password

The account name is arbitrary and for your use only. The password will be used to encrypt this account's information. You will need to re-enter it when using the account for any kind of outgoing transaction or when using it to cryptographically sign a message.

Note that this password does NOT protect your seed phrase. If someone knows the twelve words in your mnemonic seed, they still have control over your account even if they do not know the password.

![](../../.gitbook/assets/screen-shot-2021-05-14-at-4.54.44-pm.png)

### Set Address for Acala Mainnet

Now we will ensure that the addresses are displayed as Acala mainnet addresses.

Click on "Options" at the top-right corner of the plugin window, and under "Display address format for" select "Acala".

**Your address's format is only visual** - the data used to derive this representation of your address are the same, so **you can use the same address on multiple chains**. 

You can copy your address by clicking on the account's icon.

![](../../.gitbook/assets/screen-shot-2021-05-14-at-4.58.59-pm.png)

### Set Address for Karura Mainnet

Click on "Options" at the top-right corner of the plugin window, and under "Display address format for" select "Acala".

**Your address's format is only visual** - the data used to derive this representation of your address are the same, so **you can use the same address on multiple chains**. 

You can copy your address by clicking on the account's icon.

![](../../.gitbook/assets/screen-shot-2021-05-14-at-4.59.11-pm.png)

### Set Address for Polkadot Mainnet

Click on "Options" at the top-right corner of the plugin window, and under "Display address format for" select "Acala".

**Your address's format is only visual** - the data used to derive this representation of your address are the same, so **you can use the same address on multiple chains**. 

You can copy your address by clicking on the account's icon.

![](../../.gitbook/assets/screen-shot-2021-05-16-at-9.45.59-am.png)

### Set Address for Kusama Mainnet

Click on "Options" at the top of the plugin window, and under "Display address format for" select "Acala".

**Your address's format is only visual** - the data used to derive this representation of your address are the same, so **you can use the same address on multiple chains**. 

You can copy your address by clicking on the account's icon.

![](../../.gitbook/assets/screen-shot-2021-05-16-at-9.46.09-am.png)

### Convert Address for different chain formats

You can use the [Subscan Address Transform tool](https://polkadot.subscan.io/tools/ss58_transform) to convert your address between the different chain formats.

Enter any address in the input box on the left-hand side, then click **`Transform`** button, you can see address formats for all chains on the right-hand side.

