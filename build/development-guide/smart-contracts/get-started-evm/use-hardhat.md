# Using Hardhat to deploy on Acala, Karura & Mandala

This example is inpired by the [Getting Started](https://hardhat.org/getting-started/) section of Hardhat Documentation.  

## Prerequisites
- node
- yarn

## Let's create a basic sample project

```bash=
mkdir hardhat && cd hardhat
yarn add hardhat
yarn hardhat
```

```bash=
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

Welcome to Hardhat v2.6.8

✔ What do you want to do? · Create a basic sample project
✔ Hardhat project root: · /home/.../hardhat
✔ Do you want to add a .gitignore? (Y/n) · y
✔ Do you want to install this sample project's dependencies with yarn (@nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers)? (Y/n) · y
```

## Add the RPC nodes

Add the networks section to the `hardhat.config.js` file inside the `module.exports` like so:

```javascript=
module.exports = {
  solidity: "0.8.4",
  networks: {
    development: {
      url: 'http://localhost:8545',
      chainId: 595,
      gasPrice: 429496729610000,
      // Development built-in default deployment account
      accounts: ["0xa872f6cbd25a0e04a08b1e21098017a9e6194d101d75e13111f71410c59cd57f"]
    }
  }
}
```


## To compile it, simply run:

`yarn hardhat compile`
```
Compiling 2 files with 0.8.4
Compilation finished successfully
```

## You can run your tests with:

`yarn hardhat test --network development`
```
  Greeter
    ✓ Should return the new greeting once it's changed (554ms)


  1 passing (555ms)
```

## Next, to deploy the contract we will use a Hardhat script:

`yarn hardhat run scripts/sample-script.js --network development`
```
Greeter deployed to: 0x3d3593927228553b349767ABa68d4fb1514678CB
```

## Going beyond and reading the on-chain data

Using the hardhat console, you can interact with your deployed contracts.  

`yarn hardhat console --network development`
```
Welcome to Node.js v14.18.1.
Type ".help" for more information.
> const Greeter = await ethers.getContractFactory("Greeter");
undefined
> const greeter = await Greeter.attach('0x3d3593927228553b349767ABa68d4fb1514678CB')
undefined
> await greeter.greet()
'Hello, Hardhat!'
```
