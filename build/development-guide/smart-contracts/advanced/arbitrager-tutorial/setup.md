# Arbitrager Tutorial

In this tutorial we're going to be utilizing Acala's on chain scheduler and oracle to create an
automatic arbitrager that swaps a pair of tokens with a goal of making the biggest possible profit.

## Setup

We're going to be creating a project from scratch building it with using Waffle to allow us to
create tests later in the tutorial. If you'd rather build your smart contracts using Remix check out
this page [https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/use-remix](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/use-remix).

### 1. Install Depencencies

To start, lets install nodejs and yarn if you haven't already:

```text
sudo apt install -y nodejs

npm install --global yarn
```

Next, run the following commands in the folder where you want your project to live:

```text
mkdir arbitrager-contract
cd arbitrager-contract

yarn init -y
yarn add --dev ethereum-waffle@3.4.0
yarn add --dev @openzeppelin/contracts@^3.4.0
yarn add @uniswap/v2-periphery@1.1.0-beta.0
yarn add @acala-network/contracts@1.0.23
```

And add the following script (place the code within `scripts` section) to your `package.json`:

```text
"build": "waffle waffle.json"
```

This will create a folder for our project, initialize a new yarn project, and add waffle and other
required dependencies for this tutorial. We'll be using waffle to compile our smart contracts.

Within your project folder, create a file called `waffle.json` and paste the following inside the
file:

```text
{
  "compilerType": "solcjs",
  "compilerVersion": "0.6.6",
  "sourceDirectory": "./contracts",
  "outputDirectory": "./build"
}
```

This file is the configurations for `waffle` to use when building our smart contracts.

Finally let's create our `contracts` folder.

```text
mkdir contracts
```
