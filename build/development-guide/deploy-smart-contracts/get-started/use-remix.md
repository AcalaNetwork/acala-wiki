# Use Remix

There are multiple tools you can use to develop and compile Solidity contracts, we'd present two here as options

* online web app Remix 
* Solidity development and testing framework Waffle

### Compile a Solidity Contract using Remix Comment

This guide walks through the process of creating and deploying a Solidity-based smart contract to the Acala standalone node using the [Remix](http://remix.ethereum.org/). Remix is one of the commonly used development environments for smart contracts on Ethereum.

### **1. Launch Remix**

Navigate to [https://remix.ethereum.org/](https://remix.ethereum.org/). Under `Environments`, select `Solidity` to configure Remix for Solidity development, then navigate to the `File Explorers` view.

Here’s an example to compile an ERC20 contract using Remix.
Open Remix and under the `File` section click `New File`. 
![](https://i.imgur.com/J9jtCF4.png)

In the file explorer in the left window will appear an input, where you write filename: `BasicToken.sol`.

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
![](https://i.imgur.com/le9ZroU.png)

Then click the `Solidity compiler` on the left sidebar sidebar, make sure that you use exactly same compiler version that is shown in above screenshot; and click the `Compile BasicToken.sol` button. 

Remix will all of the Open Zeppelin dependencies and compile the contract.

### **3. Get the ABI & bytecode File**

To deploy smart contract into Acala EVM we will need a file with ABI (Application Binary Interface) - metadata of smart contract, allowing to interact with it; and bytecode - the compiled code which will be executed. For Acala EVM we need to have both of these data in one file.

Navigate back to `File explorers` (the top icon in the left sidebar), in the opened explorer select folder `artifacts`, and find inside the `BasicToken.json` file, click on it. You will see the content of the file, copy this content.
![](https://i.imgur.com/SHH7Mj3.png)

Copy and paste the content and save it locally, this is the ABI & bytecode file that will be deployed to Acala EVM later.


####   <a id="Compile-a-Solidity-Contract-using-Remix"></a>

