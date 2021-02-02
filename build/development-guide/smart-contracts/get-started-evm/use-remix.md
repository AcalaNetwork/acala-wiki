# Use Remix

There are multiple tools you can use to develop and compile Solidity contracts, we'd present two here as options:

* online web app Remix 
* Solidity development and testing framework Waffle

### Compile a Solidity Contract using Remix Comment

This guide walks through the process of creating and deploying a Solidity-based smart contract to the Acala standalone node using the [Remix](http://remix.ethereum.org/). Remix is one of the commonly used development environments for smart contracts on Ethereum.

### **1. Launch Remix**

Navigate to [https://remix.ethereum.org/](https://remix.ethereum.org/). Under `Environments`, select `Solidity` to configure Remix for Solidity development, then navigate to the `File Explorers` view.

Here’s an example to compile an ERC20 contract using Remix.

![](https://i.imgur.com/82B9QJA.png)

Hit the `+` button under the `File Explorers` to create a new Solidity file. 

Enter the name `BasicToken.sol` into the popup dialogue.

### **2. Compile the Solidity code**

Paste the following code into the editor tab that comes up.

```text
pragma solidity ^0.7.0;

import 'https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.2.0-solc-0.7/contracts/token/ERC20/ERC20.sol';

// This ERC-20 contract mints the specified amount of tokens to the contract creator.
contract BasicToken is ERC20 {
  constructor(uint256 initialSupply) ERC20("BASICT", "BAT") public {
    _mint(msg.sender, initialSupply);
  }
}

```

Note: this is a simple ERC-20 contract based on the Open Zeppelin ERC-20 template. On construction, it creates the BasicToken with the symbol BAT, and mints the total initial supply.

Below is the editor view.

![](https://i.imgur.com/mdZhs7y.png)

Then select `Solidity compiler` on the sidebar, and press the `Compile BasicToken.sol` button. 

Remix downloads all of the Open Zeppelin dependencies and compiles the contract.

### **3. Get the ABI File**

Navigate back to `File explorers` , in the `artifacts` section find the `BasicToken.json` file. Copy and paste the content and save it locally, this is the ABI file that will be deployed to Acala EVM later.

![](https://i.imgur.com/qzonFHr.png)

####   <a id="Compile-a-Solidity-Contract-using-Remix"></a>

