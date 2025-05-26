# Writing the Smart Contract

The smart contract we're writing is a basic automatic arbitrager. The user can transfer a pair of
tokens to the Arbitrager smart contract and the arbitrager will use the on-chain Oracle to retrieve
token prices and use periodic calls from on-chain Scheduler to swap the tokens at Uniswap to
accumulate the biggest possible value.

Inside the `contracts` folder create `Arbitrager.sol` and paste the following code:

```text
pragma solidity ^0.6.0;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

import '@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router01.sol';

contract Arbitrager {
    address public immutable factory;
    IUniswapV2Router01 public immutable router;
    IERC20 public immutable tokenA;
    IERC20 public immutable tokenB;

    constructor(
        address factory_,
        IUniswapV2Router01 router_,
        IERC20 tokenA_,
        IERC20 tokenB_
    )
        public
    {
        factory = factory_;
        router = router_;
        tokenA = tokenA_;
        tokenB = tokenB_;

        tokenA_.approve(address(router_), MAX_INT);
        tokenB_.approve(address(router_), MAX_INT);
    }
}
```

Here you can see we're importing the interfaces describing ERC20 and Uniswap V2 Router smart
contracts. We also create global variables for the Uniswap factory address, and the variables
pointing to the smart contracts that we imported as interfaces. In a public environment these would
already be delpoyed and we only need to provide their addresses to our Arbitrager smart contract. We
do this in the constructor.

Next lets give the highest possible approval for both tokens to the Uniswap V2 router in the
constructor. Within the contract add the following:

```text
pragma solidity ^0.6.0;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

import '@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router01.sol';

contract Arbitrager {
    address public immutable factory;
    IUniswapV2Router01 public immutable router;
    IERC20 public immutable tokenA;
    IERC20 public immutable tokenB;

    constructor(
        address factory_,
        IUniswapV2Router01 router_,
        IERC20 tokenA_,
        IERC20 tokenB_
    )
        public
    {
        factory = factory_;
        router = router_;
        tokenA = tokenA_;
        tokenB = tokenB_;

        tokenA_.approve(address(router_), MAX_INT);
        tokenB_.approve(address(router_), MAX_INT);
    }
}
```

Our work with the constructor is finished, let's continue by adding the logic to our Arbitrager's
arbitrage.

## Implementing arbitration

Let's call the function containing the arbitrage logic `trigger`. Copy the following into your smart
contract and let's break down the changes:

<details>
  <summary>Expand this section to view the current Arbitrager smart contract source code</summary>

    pragma solidity ^0.6.0;

    import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

    import '@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router01.sol';
    import '@uniswap/v2-periphery/contracts/libraries/UniswapV2Library.sol';

    import "@acala-network/contracts/oracle/IOracle.sol";
    import "@acala-network/contracts/utils/Address.sol";

    contract Arbitrager is ADDRESS {
        address public immutable factory;
        IUniswapV2Router01 public immutable router;
        IERC20 public immutable tokenA;
        IERC20 public immutable tokenB;

        constructor(
            address factory_,
            IUniswapV2Router01 router_,
            IERC20 tokenA_,
            IERC20 tokenB_
        )
            public
        {
            factory = factory_;
            router = router_;
            tokenA = tokenA_;
            tokenB = tokenB_;

            tokenA_.approve(address(router_), MAX_INT);
            tokenB_.approve(address(router_), MAX_INT);
        }

        function trigger() public {
            // Get prices of the tokens from the Oracle
            uint256 priceA = IOracle(ADDRESS.Oracle).getPrice(address(tokenA));
            uint256 priceB = IOracle(ADDRESS.Oracle).getPrice(address(tokenB));

            // Get balances of the tokens from the respective smart contracts
            uint256 balA = tokenA.balanceOf(address(this));
            uint256 balB = tokenB.balanceOf(address(this));

            // Retrieve reserve of tokens from Uniswap V2 Library smart contract
            (uint256 reserveA, uint256 reserveB) = UniswapV2Library.getReserves(
                                                                        factory,
                                                                        address(tokenA),
                                                                        address(tokenB)
                                                                    );

            // Calculate which token to get
            bool buyA;
            if (reserveA > reserveB) {
                uint256 reserveRatio = reserveA * 1000 / reserveB;
                uint256 priceRatio = priceA * 1000 / priceB;
                buyA = reserveRatio < priceRatio;
            } else {
                uint256 reserveRatio = reserveB * 1000 / reserveA;
                uint256 priceRatio = priceB * 1000 / priceA;
                buyA = reserveRatio > priceRatio;
            }
    
            // Determine the amount of the token to get
            uint256 amount;
            address[] memory path = new address[](2);
            if (buyA) {
                amount = balB / 10;
                path[0] = address(tokenB);
                path[1] = address(tokenA);
            } else {
                amount = balA / 10;
                path[0] = address(tokenA);
                path[1] = address(tokenB);
            }

            // Swap the two tokens based on the caluclations from above
            if (amount != 0) {
                router.swapExactTokensForTokens(
                            amount,
                            0,
                            path,
                            address(this),
                            now + 1
                        );
            }
        }
    }

</details>

The `triger` function utilizes the on-chain Oracle, to retrieve the prices of the tokens that we
assign to the Arbitrager. For this, we need to import the Oracle's interface in the import section
of our smart contract with `import "@acala-network/contracts/oracle/IOracle.sol";`. To interact with
the Oracle, we need to pass its address to the smart contract. Since the Oracle is one of the Acala
EVM's predeployed contracts, it is always located at the same address, which can be retrieved by the
ADDRESS utility. We imported it using the `import "@acala-network/contracts/utils/Address.sol";`
statement in the import section of the smart contract and made sure, Arbitrager is able to use it,
by configuring the inheritance by modifying the contract definition. Which is now `contract
Arbitrager is ADDRESS`.

Once Oracle interface is imported and ADDRESS utility added, we can retrieve the prices of both
tokens:
```
uint256 priceA = IOracle(ADDRESS.Oracle).getPrice(address(tokenA));
uint256 priceB = IOracle(ADDRESS.Oracle).getPrice(address(tokenB));
```

Next, we need to get balances of each tokens. Remember to transfer the tokens you whish for
Arbitrager to use to it, otherwise it can't do arbitrage. For this we query both token smart
contracts for balance:
```
uint256 balA = tokenA.balanceOf(address(this));
uint256 balB = tokenB.balanceOf(address(this));
```

Last step before calculating the arbitrage is to retrieve the reserves of the tokens at Uniswap:
```
(uint256 reserveA, uint256 reserveB) = UniswapV2Library.getReserves(
                                                            factory,
                                                            address(tokenA),
                                                            address(tokenB)
                                                        );
```

To be able to interact with the Uniswap V2 Library to get the reserves of the tokens, we need to
import it using `import '@uniswap/v2-periphery/contracts/libraries/UniswapV2Library.sol';`.

Now that we have all the information to make a decision on which token to swap for which, let's
implement a simple decision mechainc. We will require a state boolean variable to store this
decision and then assign it value based on the result of our calculation:
```
bool buyA;
if (reserveA > reserveB) {
    uint256 reserveRatio = reserveA * 1000 / reserveB;
    uint256 priceRatio = priceA * 1000 / priceB;
    buyA = reserveRatio < priceRatio;
} else {
    uint256 reserveRatio = reserveB * 1000 / reserveA;
    uint256 priceRatio = priceB * 1000 / priceA;
    buyA = reserveRatio > priceRatio;
}
```

As you can see, we created a `buyA` variable that determines whether we should swap the token B for
the token A or vice versa. We calculate the `reserveRatio` and `priceRatio`, which we use to make
the decision. For example: If token A's reserve is greater than token B's and if the `priceRatio` is
bigger than `reserveRatio`, we buy token A.

Next we need to determine the amount of token to sell. Since we are implementing a simple strategy,
the tenth of the balance of the token we are selling should suffice. We also set the `path[]` array
in this step. The `path[]` array contains the address of the token we would like to sell on index
`[0]` and the address of the token we would like to buy on index `[1]`. The code for this step is as
follows:
```
uint256 amount;
address[] memory path = new address[](2);
if (buyA) {
    amount = balB / 10;
    path[0] = address(tokenB);
    path[1] = address(tokenA);
} else {
    amount = balA / 10;
    path[0] = address(tokenA);
    path[1] = address(tokenB);
}
```

The only thing remaining is to implement the actual swap of the tokens. We do it by calling the
Uniswap V2 Router's `swapExactTokensForTokens` function, which allows us to set the exact amount of
token we would like to sell and the minimum amount of token we are prepared to buy for the other
token:
```
if (amount != 0) {
    router.swapExactTokensForTokens(
                amount,
                0,
                path,
                address(this),
                now + 1
            );
}
```

The parameters we are passing to `swapExactTokensForTokens` are:

- amount: the amount of token we want to sell (we calculated this in the step above this one)
- 0: the minimum amount of token we are willing to buy
- path: an array containig the addresses of tokens we want to sell and buy (we calculated this in
the step above)
- address(this): address of this smart contract as the address we want to send the newly bought
tokens to
- now + 1: the deadline of the swap; indicating we want to have it executed as soon as possible

This concludes our `trigger` function for now.

### Implementing Schedule

Now that our `trigger` function is operational, let's automate its execution, so we don't have to
manually call it. We will be using the on-chain Schedule smart contract for this:
<details>
  <summary>Expand this section to view the current Arbitrager smart contract source code</summary>

    pragma solidity ^0.6.0;

    import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

    import '@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router01.sol';
    import '@uniswap/v2-periphery/contracts/libraries/UniswapV2Library.sol';

    import "@acala-network/contracts/oracle/IOracle.sol";
    import "@acala-network/contracts/schedule/ISchedule.sol";
    import "@acala-network/contracts/utils/Address.sol";

    contract Arbitrager is ADDRESS {
        address public immutable factory;
        IUniswapV2Router01 public immutable router;
        IERC20 public immutable tokenA;
        IERC20 public immutable tokenB;
        uint256 public period;
        bytes public schedulerTaskId;

        constructor(
            address factory_,
            IUniswapV2Router01 router_,
            IERC20 tokenA_,
            IERC20 tokenB_
        )
            public
        {
            factory = factory_;
            router = router_;
            tokenA = tokenA_;
            tokenB = tokenB_;

            tokenA_.approve(address(router_), MAX_INT);
            tokenB_.approve(address(router_), MAX_INT);
        }

        function scheduleTriggerCall(uint period_) public returns(bool){
            require(keccak256(schedulerTaskId) == keccak256(bytes("")), "The call is already schdeuled!");

            period = period_;

            require(ISchedule(ADDRESS.Schedule).scheduleCall(
                                                    address(this),
                                                    0,
                                                    1000000,
                                                    5000,
                                                    period_,
                                                    abi.encodeWithSignature("trigger()")
                                                ));
            return true;
        }

        function setTaskId(bytes memory task_id_) public returns(bool) {
            require(keccak256(schedulerTaskId) == keccak256(bytes("")), "task_id is already set!");

            schedulerTaskId = task_id_;
            return true;
        }

        function trigger() public {
            // Get prices of the tokens from the Oracle
            uint256 priceA = IOracle(ADDRESS.Oracle).getPrice(address(tokenA));
            uint256 priceB = IOracle(ADDRESS.Oracle).getPrice(address(tokenB));

            // Get balances of the tokens from the respective smart contracts
            uint256 balA = tokenA.balanceOf(address(this));
            uint256 balB = tokenB.balanceOf(address(this));

            // Retrieve reserve of tokens from Uniswap V2 Library smart contract
            (uint256 reserveA, uint256 reserveB) = UniswapV2Library.getReserves(
                                                                        factory,
                                                                        address(tokenA),
                                                                        address(tokenB)
                                                                    );

            // Calculate which token to get
            bool buyA;
            if (reserveA > reserveB) {
                uint256 reserveRatio = reserveA * 1000 / reserveB;
                uint256 priceRatio = priceA * 1000 / priceB;
                buyA = reserveRatio < priceRatio;
            } else {
                uint256 reserveRatio = reserveB * 1000 / reserveA;
                uint256 priceRatio = priceB * 1000 / priceA;
                buyA = reserveRatio > priceRatio;
            }
    
            // Determine the amount of the token to get
            uint256 amount;
            address[] memory path = new address[](2);
            if (buyA) {
                amount = balB / 10;
                path[0] = address(tokenB);
                path[1] = address(tokenA);
            } else {
                amount = balA / 10;
                path[0] = address(tokenA);
                path[1] = address(tokenB);
            }

            // Swap the two tokens based on the caluclations from above
            if (amount != 0) {
                router.swapExactTokensForTokens(
                            amount,
                            0,
                            path,
                            address(this),
                            now + 1
                        );
            }
        }
    }
</details>

First we imported the Schedule smart contract's interface using `import
"@acala-network/contracts/schedule/ISchedule.sol";`. We added the `period` and `schedulerTaskId`
storage variables. The `period` variable will store the information about the minimum amount of
blocks the Schedule should wait before exectuting the call that we configured, and the
`schedulerTaskId` variable will store the `task_id` assigned to our call for us to be able to modify
the call schedule.

Two additional functions were added as well. `scheduleTriggerCall` schedules the call of the
`trigger` function with the Schedule smart contract. It accepts the `period_` parameter which
determines how many blocks the Schedule should wait, before executing the call:
```
function scheduleTriggerCall(uint period_) public returns(bool){
    require(keccak256(schedulerTaskId) == keccak256(bytes("")), "The call is already schdeuled!");

    period = period_;

    require(ISchedule(ADDRESS.Schedule).scheduleCall(
                                            address(this),
                                            0,
                                            1000000,
                                            5000,
                                            period_,
                                            abi.encodeWithSignature("trigger()")
                                        ));
    return true;
}
```

You can see that the ADDRESS utility is used once more, this time, to retrieve the address of the
Schedule smart contract. If you whish to read more about the Schedule smart contract, please refer
to its [wiki entry](https://wiki.acala.network/build/development-guide/smart-contracts/advanced/use-on-chain-scheduler).
The arguments passed to it are hardcoded and the only variable is `period_`.

`setTaskId` function that has been added as well is used to store the `task_id` that Schedule smart
contract uses to track the call we registered with it. It allows us to store this `task_id` into a
`schedulerTaskId` storage variable and use it to simplify the manipulation of the call:
```
function setTaskId(bytes memory task_id_) public returns(bool) {
    require(keccak256(schedulerTaskId) == keccak256(bytes("")), "task_id is already set!");

    schedulerTaskId = task_id_;
    return true;
}
```

We also added a guard clause to make sure, we are not overwriting an already existing `task_id`.

Currently our setup allows us to have one scheduled call sometime in the future, but we still have
to call the `scheduleTriggerCall` again to shedule a new call once the first one executes. Let's
change that. We will add a call to Schedule to `trigger` in order to allow the `trigger` to be
called periodically over an unlimited period of time:
<details>
  <summary>Expand this section to view the current Arbitrager smart contract source code</summary>

    pragma solidity ^0.6.0;

    import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

    import '@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router01.sol';
    import '@uniswap/v2-periphery/contracts/libraries/UniswapV2Library.sol';

    import "@acala-network/contracts/oracle/IOracle.sol";
    import "@acala-network/contracts/schedule/ISchedule.sol";
    import "@acala-network/contracts/utils/Address.sol";

    contract Arbitrager is ADDRESS {
        address public immutable factory;
        IUniswapV2Router01 public immutable router;
        IERC20 public immutable tokenA;
        IERC20 public immutable tokenB;
        uint256 public period;
        bytes public schedulerTaskId;

        constructor(
            address factory_,
            IUniswapV2Router01 router_,
            IERC20 tokenA_,
            IERC20 tokenB_
        )
            public
        {
            factory = factory_;
            router = router_;
            tokenA = tokenA_;
            tokenB = tokenB_;

            tokenA_.approve(address(router_), MAX_INT);
            tokenB_.approve(address(router_), MAX_INT);
        }

        function scheduleTriggerCall(uint period_) public returns(bool){
            require(keccak256(schedulerTaskId) == keccak256(bytes("")), "The call is already schdeuled!");

            period = period_;

            require(ISchedule(ADDRESS.Schedule).scheduleCall(
                                                    address(this),
                                                    0,
                                                    1000000,
                                                    5000,
                                                    period_,
                                                    abi.encodeWithSignature("trigger()")
                                                ));
            return true;
        }

        function setTaskId(bytes memory task_id_) public returns(bool) {
            require(keccak256(schedulerTaskId) == keccak256(bytes("")), "task_id is already set!");

            schedulerTaskId = task_id_;
            return true;
        }

        function trigger() public {
            require(msg.sender == address(this), "Can only be called by this smart contract.");
            // Schedule another call with Scheduler
            ISchedule(ADDRESS.Schedule).scheduleCall(
                                            address(this),
                                            0,
                                            1000000,
                                            5000,
                                            period,
                                            abi.encodeWithSignature("trigger()")
                                        );
            
            // Get prices of the tokens from the Oracle
            uint256 priceA = IOracle(ADDRESS.Oracle).getPrice(address(tokenA));
            uint256 priceB = IOracle(ADDRESS.Oracle).getPrice(address(tokenB));

            // Get balances of the tokens from the respective smart contracts
            uint256 balA = tokenA.balanceOf(address(this));
            uint256 balB = tokenB.balanceOf(address(this));

            // Retrieve reserve of tokens from Uniswap V2 Library smart contract
            (uint256 reserveA, uint256 reserveB) = UniswapV2Library.getReserves(
                                                                        factory,
                                                                        address(tokenA),
                                                                        address(tokenB)
                                                                    );

            // Calculate which token to get
            bool buyA;
            if (reserveA > reserveB) {
                uint256 reserveRatio = reserveA * 1000 / reserveB;
                uint256 priceRatio = priceA * 1000 / priceB;
                buyA = reserveRatio < priceRatio;
            } else {
                uint256 reserveRatio = reserveB * 1000 / reserveA;
                uint256 priceRatio = priceB * 1000 / priceA;
                buyA = reserveRatio > priceRatio;
            }
    
            // Determine the amount of the token to get
            uint256 amount;
            address[] memory path = new address[](2);
            if (buyA) {
                amount = balB / 10;
                path[0] = address(tokenB);
                path[1] = address(tokenA);
            } else {
                amount = balA / 10;
                path[0] = address(tokenA);
                path[1] = address(tokenB);
            }

            // Swap the two tokens based on the caluclations from above
            if (amount != 0) {
                router.swapExactTokensForTokens(
                            amount,
                            0,
                            path,
                            address(this),
                            now + 1
                        );
            }
        }
    }
</details>

You might have noticed that we added a guard clause to the `trigger`function to ensure it is only
called by the Arbitrager smart contract. This protects it from being called by someone else, which
could potentially cause the Arbitrager to execute `trigger` function every block or even multiple
times per block. The call to Schedule within the `trigger` is identical to the one in
`scheduleTriggerCall`. The only difference betwee these two calls is the use of storage or state
variable:
```
ISchedule(ADDRESS.Schedule).scheduleCall(
                                address(this),
                                0,
                                1000000,
                                5000,
                                period,
                                abi.encodeWithSignature("trigger()")
                            );
```

For the final stretch of Arbitrager development, let's add two more functions. To be able to change
the period of the call from Schedule and to stop the call from Schedule:

<details>
  <summary>Expand this section to view the current Arbitrager smart contract source code</summary>

    pragma solidity ^0.6.0;

    import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

    import '@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router01.sol';
    import '@uniswap/v2-periphery/contracts/libraries/UniswapV2Library.sol';

    import "@acala-network/contracts/oracle/IOracle.sol";
    import "@acala-network/contracts/schedule/ISchedule.sol";
    import "@acala-network/contracts/utils/Address.sol";

    contract Arbitrager is ADDRESS {
        address public immutable factory;
        IUniswapV2Router01 public immutable router;
        IERC20 public immutable tokenA;
        IERC20 public immutable tokenB;
        uint256 public period;
        bytes public schedulerTaskId;

        constructor(
            address factory_,
            IUniswapV2Router01 router_,
            IERC20 tokenA_,
            IERC20 tokenB_
        )
            public
        {
            factory = factory_;
            router = router_;
            tokenA = tokenA_;
            tokenB = tokenB_;

            tokenA_.approve(address(router_), MAX_INT);
            tokenB_.approve(address(router_), MAX_INT);
        }

        function scheduleTriggerCall(uint period_) public returns(bool){
            require(keccak256(schedulerTaskId) == keccak256(bytes("")), "The call is already schdeuled!");

            period = period_;

            require(ISchedule(ADDRESS.Schedule).scheduleCall(
                                                    address(this),
                                                    0,
                                                    1000000,
                                                    5000,
                                                    period_,
                                                    abi.encodeWithSignature("trigger()")
                                                ));
            return true;
        }

        function setTaskId(bytes memory task_id_) public returns(bool) {
            require(keccak256(schedulerTaskId) == keccak256(bytes("")), "task_id is already set!");

            schedulerTaskId = task_id_;
            return true;
        }

        function rescheduleTriggerCall(uint newPeriod) public returns(bool){
            period = newPeriod;
            require(ISchedule(ADDRESS.Schedule).rescheduleCall(newPeriod, schedulerTaskId));
            return true;
        }

        function cancelTriggerCall() public returns(bool){
            require(ISchedule(ADDRESS.Schedule).cancelCall(schedulerTaskId));
            delete schedulerTaskId;
            return true;
        }

        function trigger() public {
            require(msg.sender == address(this), "Can only be called by this smart contract.");
            // Schedule another call with Scheduler
            ISchedule(ADDRESS.Schedule).scheduleCall(
                                            address(this),
                                            0,
                                            1000000,
                                            5000,
                                            period,
                                            abi.encodeWithSignature("trigger()")
                                        );
            
            // Get prices of the tokens from the Oracle
            uint256 priceA = IOracle(ADDRESS.Oracle).getPrice(address(tokenA));
            uint256 priceB = IOracle(ADDRESS.Oracle).getPrice(address(tokenB));

            // Get balances of the tokens from the respective smart contracts
            uint256 balA = tokenA.balanceOf(address(this));
            uint256 balB = tokenB.balanceOf(address(this));

            // Retrieve reserve of tokens from Uniswap V2 Library smart contract
            (uint256 reserveA, uint256 reserveB) = UniswapV2Library.getReserves(
                                                                        factory,
                                                                        address(tokenA),
                                                                        address(tokenB)
                                                                    );

            // Calculate which token to get
            bool buyA;
            if (reserveA > reserveB) {
                uint256 reserveRatio = reserveA * 1000 / reserveB;
                uint256 priceRatio = priceA * 1000 / priceB;
                buyA = reserveRatio < priceRatio;
            } else {
                uint256 reserveRatio = reserveB * 1000 / reserveA;
                uint256 priceRatio = priceB * 1000 / priceA;
                buyA = reserveRatio > priceRatio;
            }
    
            // Determine the amount of the token to get
            uint256 amount;
            address[] memory path = new address[](2);
            if (buyA) {
                amount = balB / 10;
                path[0] = address(tokenB);
                path[1] = address(tokenA);
            } else {
                amount = balA / 10;
                path[0] = address(tokenA);
                path[1] = address(tokenB);
            }

            // Swap the two tokens based on the caluclations from above
            if (amount != 0) {
                router.swapExactTokensForTokens(
                            amount,
                            0,
                            path,
                            address(this),
                            now + 1
                        );
            }
        }
    }
</details>

`rescheduleTriggerCall` accepts one parameter called `newPeriod` and interacts with Schedule smart
contract to change the period in which the call of `trigger` function is executed. It uses the
storage variable `schedulerTaskId` in order to provide reference which event it is changing:
```
function rescheduleTriggerCall(uint newPeriod) public returns(bool){
    period = newPeriod;
    require(ISchedule(ADDRESS.Schedule).rescheduleCall(newPeriod, schedulerTaskId));
    return true;
}
```

As you can see the `rescheduleTriggerCall` also changes the state of `period` storage variable and
assigns it the `newPeriod` value in order for `trigger` to use the newly assigned period when
scheduling calls by itself.

`cancelTriggerCall` function is used to stop the calls form Schedule smart contract. Once it is
called we also clear the `schedulerTaskId` variable in order to be able to call
`scheduleTriggerCall` sometime in the future and save the new `task_id` using `setTaskId` function:
```
function cancelTriggerCall() public returns(bool){
    require(ISchedule(ADDRESS.Schedule).cancelCall(schedulerTaskId));
    delete schedulerTaskId;
    return true;
}
```

And that's it! We now have a fully functioning Arbitrager smart contract with automated trigger
calls!

**Please note that for this contract to be used in production environment, the arbitrage logic**
**would need to be refined, as the logic currently implemented is very simple and meant as an**
**example. Additionally access control should be implemented in all of the functions ecxept**
**`trigger`.**

## Building

To build the smart contract run the following in the project's root directory:

```text
yarn build
```

## Deploying

To deploy the smart contract follow the directions [here](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/deploy-contracts) or you can check out the deploy script of Arbitrager example [here](https://github.com/AcalaNetwork/evm-examples/blob/master/arbitrager/src/deploy.ts).
