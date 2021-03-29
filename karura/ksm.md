# KSM Address

# Overview: Getting KSM address, creating a Wallet, Connecting to the Acala Testnet
To connect to Acala, users must first have a Kusama address (account). Users can attain an address by setting up a wallet. The wallet and address can then be used to interface with the Acala testnet.

To connect to Acala, users must first have a Kusama address (account). Users can attain an address by setting up a wallet. The wallet and address can then be used to interface with the Acala testnet.

If you already have a DOT or other Substrate-based chain address e.g. Acala address, this is how to get the corresponding KSM address.
Otherwise, check how to create a new wallet.

# Getting a corresponding KSM address from the existing wallet

There are two options how you can get your KSM address: 
- you can use [Subscan Address Transform](https://acala-testnet.subscan.io/tools/ss58_transform);
- setting your wallets in polkadot.js extension or PolkaWallet.

### Using Subscan transform

1. Navigate to [Subscan Address Transform](https://acala-testnet.subscan.io/tools/ss58_transform) and paste your existing account address into "Input Account or Public Key".

![](https://i.imgur.com/v7damrj.png)

2. Press "Transform" and find the corresponding Kusama address in the appeared list on the right (in the screenshot, it is third from the top).

![](https://i.imgur.com/bv0T6dD.png)

### Using Polkadot.js extension

1. Open polkadot.js extension in your browser and press 3 dots on the right from your account name.
2. In the opened window click on the dropdown menu and pick "Kusama Relay Chain"
![](https://i.imgur.com/IVZqsAR.png)
3. Now all your accounts are converted to Kusama format, you can copy them.

### Using Polkawallet

1. Open PolkaWallet on your mobile device and click the menu button on the top-right corner.
![](https://i.imgur.com/z7uFoCj.jpg =250x)

2. In the opened menu select Kusama logo (second from the top) and press on the appeared address on the main screen.
![](https://i.imgur.com/chGwQDP.jpg =250x)
3. Click on the account block under the "Add Account" button, which will navigate you back to the main page.
![](https://i.imgur.com/Btjlla4.jpg =250x)
4. Now your wallet is set up and you can copy your Kusama address. You can see that your wallet changed color to black and among your assets you can see KSM.

# Creating a new Wallet

While there are several wallets that support Kusama, **we've found that the Polkadot.{js} browser extension best serves most desktop users and the Polkawallet mobile app best serves most mobile users.** Follow the guides below based on your wallet preference.

## Step 1: Creating a wallet through Polkadot{.js} browser extension
1) Type polkadot.js.org/extension in the address bar of your web browser.

    ![](https://i.imgur.com/YO0m3tP.png)

2) Select download for your respective browser and follow the download prompts. Brave users should click on the "Download for Chrome" button. 

    Chrome and Brave users will know if the download was successful when they click on the Extension button (puzzle piece) in the right-hand corner of their browser and the orange circle with a "P" appears. Chrome users can pin the extension into their toolbar by clicking on the pin icon (see screenshot). Firefox users will know if the download was successful because the orange circle with a "P" will appear in the top right-hand corner of their browser.

    ![](https://i.imgur.com/pkZuAht.png)

3) Click on the Polkadot logo in your browser. A welcome dialogue box will appear. Click "Understood, let me continue."

4) Click on the "+" in the center of the circle to add an account.

    ![](https://i.imgur.com/sh0WAqQ.jpg)

5) A box will appear with your address along with your mnemonic seed phrase. Record the mnemonic phrase and store it somewhere safe. Don't share it with anyone. Your seed phrase can be used to recover your account if you forget your password or if you want to import your account.

    Click "I have saved my mnemonic seed safely" and proceed to the next step.

    ![](https://i.imgur.com/q5facgU.png)

6) The next prompt will allow you to select a network, name your account and create a password for the account. Because you are creating a Kusama account, select the  "Kusama Relay Chain" from the dropdown box. Also ensure that you use a strong password (at least 6 characters).

    Click on the "Add the account with the generated seed" button to create your account.

    ![](https://i.imgur.com/480oGUb.png)

7) Your Polkadot.{js} browser extension is now set up! You should see your account in the browser extension. 
    
    ![](https://i.imgur.com/X7COwhL.png)

## Step 2: Managing your account through the Polkadot UI
1) Now it's time to connect the browser extension to the Polkadot-JS UI (user interface) to manage your account.

    Type "polkadot.js.org/apps" in your search bar. 
        
2) A dialogue box will appear asking you to authorize website access. Click on the button that says "Yes, allow this application access."

    ![](https://i.imgur.com/9yHKL2f.png)

3) To switch networks, look towards the left-hand side of the navigation ribbon (pink in this example) and select the dropdown near the network logo (Polkadot logo in this example).

    Click on "Kusama" and select the option "hosted by Parity." Click on the pink "Switch" button.

    ![](https://i.imgur.com/aF6aqn4.png)

4) You should see the account you established in your Polkadot.{js} extension listed in the accounts. You can use the Polkadot UI to send KSM, stake and participate in governance. Consult our other guides on these topics if you are interested in any of those areas.

    ![](https://i.imgur.com/p5m0D1J.png)
    
## Step 3: Connecting to Acala testnet
1) Type in [https://apps.acala.network](https://) in the address bar of your browser.

2) Click on the wallet icon in the navigation bar on the left-hand side. Your account details should populate. You've now successfully created an account and connected to Acala! 

    ![](https://i.imgur.com/nUyPiUE.png)

## Step 4: Exploring the Acala testnet
1) The best way to explore the Acala testnet is to actually use it! You can collect testnet tokens by clicking on the faucet icon (blue circle) at the bottom of the navigation bar on the left-hand side of the screen.

    ![](https://i.imgur.com/PwsGljl.png)

2) You will be prompted to connect to the Acala server on Discord. Enter your information.

3) Navigate to the "acala-testnet-faucet" channel.

    ![](https://i.imgur.com/4BkRnHa.png)

4) Click on the channel. Copy your address from the Acala testnet (should begin with a "5"). Type in "!drip" and your address into the faucet chat on discord.

    ![](https://i.imgur.com/3NKIKnB.png)

5) Return to the Acala testnet on your browser. Your account should be credited with testnet tokens.

    **Note that tokens on the testnet are for testing purposes only and have no real value.**
    
    ![](https://i.imgur.com/w6y3yWn.png)

6) You are now ready to test Acala's features! Select a feature to explore (e.g., Borrow aUSD, Swap, Earn, Liquid Staking) and follow the prompts to learn more.

## Step 1: Creating a wallet through Polkawallet
1) Download the Polkawallet app through the Apple App Store for iOS devices and Google Play for Android devices.

2) Click on the "Create Account" button.

    ![](https://i.imgur.com/ul163Lo.jpg)

3) A new screen will appear explaining the importance of recording your mnemonic phrase in a safe place. Click the "Next" button.
    
    ![](https://i.imgur.com/hyl3FDb.jpg)

4) Your mnemonic phrase will appear. Write the mnemonic on a piece of paper and store it somewhere safe. Click on the "Next" button.
    
    ![](https://i.imgur.com/MkzXnjg.jpg)

5) Confirm your mnemonic by entering the words in the correct order. Click on the "Next" button when completed.
    
    ![](https://i.imgur.com/EAuT7R7.jpg)

6) Name your account and create a strong password (at least 6 characters). Click on the "Next" button when completed.
    
    ![](https://i.imgur.com/3ouTHum.jpg)

7) Your wallet is now set up! The screen will default to the Polkadot network. You can determine which network you're connected to by looking at the grey text under the account name. In the case of this screenshot, it says "Polkadot."
    
    Click on the hamburger (three lines) in the right-hand corner.

    ![](https://i.imgur.com/YYYj1IN.jpg)

8) A series of logos showing the networks that you can connect to will appear on the left-hand side. Select the Kusama logo (circle with a bird) and then click on the box with the account name.
    
    ![](https://i.imgur.com/XVfCuyy.jpg)

9) The page will reload, and you will be connected to the Kusama network.
   
   ![](https://i.imgur.com/wy8GGsm.jpg)
     
## Step 2: Connecting to the Acala testnet
1) Click on the hamburger (three lines) in the right-hand corner to return to the screen where you can see the available networks.

2) Select the Acala logo (red "A") and then click on the box with the account name. 
    
    ![](https://i.imgur.com/z7uK9l8.jpg)

3) The page will refresh, and you will be connected to the Acala testnet. You've now successfully created an account and connected to Acala! 

    ![](https://i.imgur.com/AiFTJIF.jpg)

### Step 3: Exploring the Acala testnet
1) The best way to explore the Acala testnet is to actually use it! You can collect testnet tokens by clicking on "Acala" (coin icon) in the navigation bar at the bottom.

2) You will be brought to a page showing Acala's features. Click on the blue "Faucet" button at the bottom.

    ![](https://i.imgur.com/3poQRNY.jpg)

3) Click on "Assets" in the navigation bar to see that the testnet tokens have been added to your wallet. 

    **Note that tokens on the testnet are for testing purposes only and have no real value.**

    ![](https://i.imgur.com/p2T08CR.jpg)

4) Return to the Acala page by clicking on "Acala" in the navigation bar at the bottom. You are now ready to test Acala's features. Select a feature to explore (e.g., Self-Serviced Loan, Swap, Deposit & Earn, Liquid DOT) and follow the prompts to learn more.

    ![](https://i.imgur.com/fJsGY51.jpg)

