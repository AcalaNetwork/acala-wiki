## Deploy Uniswap on Acala EVM

In this tutorial, we will see how easy is to deploy existing Uniswap contracts on Acala EVM and how to communicate with them using Acala EVM Playground.

We will deploy Uniswap Factory and Router and add Liquidity for ACA- DOT token pair, using pre-deployed tokens smart contracts. After deployment, we will test contracts swapping tokens.

All the source code is located here: [evm-examples/uniswap](https://github.com/AcalaNetwork/evm-examples/tree/master/uniswap).

### Setup

First of all, ensure that you have installed `nodejs` and `yarn`. You can install them running the script:
```bash=
sudo apt install -y nodejs

npm install --global yarn
```
Next, let's clone the repository and navigate to `uniswap` folder

```bash=
git clone https://github.com/AcalaNetwork/evm-examples.git
cd evm-examples/uniswap
```

Install all required dependencies running the command:
```bash=
yarn
```

> :warning: for passing this tutorial you need to run Acala **locally**, either from the built Acala project or from the latest Docker image. You can follow the instructions here: [Connect to Node](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/connect-to-a-node).

### Deploying smart contracts

In folder `evm-examples/uniswap/artifacts` we have pre-compiled ABI & bytecode of smart contracts, that we will deploy using [src/deploy.ts](https://github.com/AcalaNetwork/evm-examples/blob/master/uniswap/src/deploy.ts) script.

Let's break down the deploy script.

First, we import compiled ABI & bytocode from `artifacts`:
```javascript=
import UniswapFactory from "../artifacts/UniswapV2Factory.json";
import UniswapRouter from "../artifacts/UniswapV2Router02.json";
```
We also need to have ABI of ERC-20 token. We don't need a bytecode, because we gonna use only its interface to connect to pre-deployed Acala Native tokens contracts:
```javascript=
import IERC20 from "../artifacts/IERC20.json";
```

The variable `ADDRESS` holds all of the pre-deployed Acala tokens addresses:
```javascript=
import ADDRESS from "@acala-network/contracts/utils/Address";
```

Here we are creating smart contract objects, using interface from ABI, address of ACA & DOT tokens smart contracts and connect our wallet:
```javascript=
const tokenACA = new Contract(ADDRESS.ACA, IERC20.abi, wallet);
const tokenDOT = new Contract(ADDRESS.DOT, IERC20.abi, wallet);
```

Next, we need to deploy uniswap factory and router using their ABI & bytecode:
```javascript=
const factory = await ContractFactory.fromSolidity(UniswapFactory).connect(wallet).deploy(deployerAddress)
const router = await ContractFactory.fromSolidity(UniswapRouter).connect(wallet).deploy(factory.address, ADDRESS.ACA);
```

Now we have deployed the minimum of the contracts, and we can proceed to Adding Liquidity creating first Liquidity Pool.

### Adding Liquidity

First, we need to approve both tokens for value "100".
```javascript=
await tokenACA.approve(router.address, dollar.mul(100));
await tokenDOT.approve(router.address, dollar.mul(100));
```

And we can finally add liquidity with proportion 2 ACA to 1 DOT:
```javascript=
await router.addLiquidity(ADDRESS.ACA, ADDRESS.DOT, dollar.mul(2), dollar, 0, 0, deployerAddress, 10000000000);
```

After it we have checks that everything works well and we have liquidity in our DOT-ACA pool:
```javascript=
const tradingPairAddress = await factory.getPair(ADDRESS.ACA, ADDRESS.DOT);
const tradingPair = new Contract(tradingPairAddress, IERC20.abi, wallet);
const lpTokenAmount = await tradingPair.balanceOf(deployerAddress);
const acaAmount = await tokenACA.balanceOf(tradingPairAddress);
const dotAmount = await tokenDOT.balanceOf(tradingPairAddress);
```

Let's run in terminal the whole script and see the results:
```bash=
yarn deploy
```

The result should look like this:
![](https://i.imgur.com/fGhQxKf.png)
We can see in logs the smart contract addresses of factory and router. Also,  see that proportion of ACA to DOT is 2:1.

As well, we can find the deployed contracts in [Acala EVM Playground](https://evm.acala.network/#/execute) and verify pools state there.

### Find deployed contracts in EVM

Navigate to [Acala EVM Playground](https://evm.acala.network/#/execute) and ensure that it's connected to your local node. Check out the settings icon on the right bottom corner. After navigate to "Execute" section in the left-side menu and at the bottom of the page click "Add an existing contract".
![](https://i.imgur.com/IX7114J.png).

In opened window paste into "Contract Address" the address of the deployed Uniswap factory that was printed to the terminal after running "deploy" script; Set contract name "Uniswap Factory"; upload ABI bundle `src/artifacts/UniswapV2Factory.json` and hit "Save" button on the bottom.
![](https://i.imgur.com/QYCJUoE.png)


In the opened tab, find `Uniswap Factory` contract and hit "Execute".
![](https://i.imgur.com/gy5PuYX.png)

Let's get the address of DOT-ADA LP, to do it, select "Message to Send" `getPair(: address : address)` and pass addresses of predeployed DOT and ACA smart contracts, which you can find in "Execute" section; and hit "Call" button.

![](https://i.imgur.com/btj95Bc.png)

On the bottom you should see the EVM address in "Call Results". The address should be the same to `tradingPair` address that we've seen in the terminal after running `deploy` script.

![](https://i.imgur.com/8N0rkrR.png)

![](https://i.imgur.com/MQgtw1w.png)

The `tradingPair` address is the address of the separate smart contract that holds in our case DOT-ACA Liquidity. We can check its balance in "Execute" section of DOT and ACA smart contracts, using the call `balanceOf` as it is shown in the screenshot below:
![](https://i.imgur.com/8ZjruE7.png)
and the result of the call:
![](https://i.imgur.com/zuTAS5Z.png)

# Swap coins

We can swap coins using (src/trade.ts)[https://github.com/AcalaNetwork/evm-examples/blob/master/uniswap/src/trade.ts] script or manually using EVM Playground. Let's do both.


### Swap coins through EVM Playground

In this example, we are going to sell 0.0000002 DOT for ACA and see how pools have changed.

To swap coins we need to "Add existing contract" of Uniswap router, as in the previous example. Upload [UniswapV2Router02.json](https://github.com/AcalaNetwork/evm-examples/blob/master/uniswap/artifacts/UniswapV2Router02.json), find router deployed address in logs in the terminal, name the contract, and press "Save".

![](https://i.imgur.com/a4YcKsn.png)

Next, let's swap tokens. Select `swapExactTokensForTokens` in "Message to Send".

> :warning: for making any calls and swaps we are going to use "Alice" development account, as it was used for adding liquidity in `deploy` script. If you want to use another account before you should execute `approve` method in ACA and DOT smart contracts.

Let's fill up all the missing arguments:
- **amountIn**: `2000000` (0.0000002 DOT without decimals)
- **amountOutMin**: `0`
- **path**: `["0x0000000000000000000000000000000001000002", "0x0000000000000000000000000000000001000000"]` addresses of smart contracts of DOT and ACA, please verify these addresses as they can be different.
- **to**: `executor's address` (Alice's address if you use this account)
- **deadline**: `10000000000` - the deadline number of block till which this transaction should be executed. We set it to a big number, meaning that we don't need a deadline.

And click "execute" on the bottom.
![](https://i.imgur.com/a0kCX67.png)

If the smart contract was executed successfully, your balance of DOTs should change, as the balance of the ACA-DOT Pool.

If you check `balanceOf` in DOTs smart contract, setting the address of ACA-DOT trading pair you should see next:
![](https://i.imgur.com/Vdy0PTK.png)
In "Call Results" we can see that amount of DOTs in the Pool is now `10000002000000`, which equals 1,0000002 DOT with decimals.

## Swap coins through code

To run swap through code we can use [src/trade.ts](https://github.com/AcalaNetwork/evm-examples/blob/master/uniswap/src/trade.ts).

On the line `8` we need to change the router address for the one that we deployed:
```javascript=
// paste your router address
const ROUTER_ADDRESS = "0x2f5abFe6621F629258460e636528da87C115A4b9";
```
The script is doing the same thing that we did manually through EVM Playground, but additionally runs `approve` on DOT and ACA smart contracts.

To run the automatic trade with:
```bash=
yarn trade
```

As result in the console, you should see the state of pools before and after the trade. It should look like this:

![](https://i.imgur.com/Ariq4SN.png)

The ACA amount of the executor changed more because ACA was used to pay transaction fees.
You can track the change in the amount of DOTs in the Liquidity pool changed after the trade.
