# Tutorial

In this tutorial, we're going to be utilizing Acala's on-chain scheduler to create an automatic subscription service that awards users more tokens the longer they're subscribed. A user can subscribe to this service, specify the subscription period, and he/she will be automatically re-subscribed. Every time he/she is "re-subscribed", the contract will reward them with some tokens. The subscription contract takes deposited funds and can automatically distribute them.

## Setup & Install Dependencies

We will create a project from scratch, build and test it using Waffle. If you'd rather build your smart contracts using Remix, please check out [this](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/use-remix) page.

To start, let's install `nodejs` and yarn if you haven't already:

```text
sudo apt install -y nodejs

npm install --global yarn
```

Next, run the following commands in the folder where you want your project to live:

```text
mkdir subscription-contract
cd subscription-contract

yarn init -y
yarn add --dev ethereum-waffle@3.2.1
```

And add the following script to your `package.json`:

```text
"build": "waffle"
```

This will create a folder for our project, initialize a new yarn project, and add waffle as a dev dependency. We'll be using waffle to compile our smart contracts.

Within your project folder, create a file called `waffle.json` and paste the following inside the file:

```text
{
  "compilerType": "solcjs",
  "compilerVersion": "0.6.2",
  "sourceDirectory": "./contracts",
  "outputDirectory": "./build"
}
```

This file is the configurations for `waffle` to use when building our smart contracts.

Finally let's create our `contracts` folder.

```text
mkdir contracts
```

## Use Scheduler.sol

Inside the `contracts` folder create `Scheduler.sol` paste the following code:

```text
pragma solidity ^0.6.0;

abstract contract Scheduler {
    // Schedule a contract call.
    function scheduleCall(
        address contract_address, // The contract address to be called in future.
        uint256 value, // How much native token to send alone with the call.
        uint256 gas_limit, // The gas limit for the call. Corresponding fee will be reserved upfront and refunded after call.
        uint256 storage_limit, // The storage limit for the call. Corresponding fee will be reserved upfront and refunded after call.
        uint256 min_delay, // Minimum number of blocks before the scheduled call will be called.
        bytes memory input_data // The input data to the call.
    )
    virtual
    public
    returns (uint256, uint256); // Returns a task id that can be used to cancel or reschedule call.
}
```

This file describes the scheduler smart contract and what arguments you must supply to it in order to use it. But how does the scheduler work?

The Acala EVM provides Solidity, Substrate, and Web3 developers a complete full-stack \(Acala+EVM+Substrate+WASM\) experience seamlessly with a single wallet. Many Substrate and WASM customization are made available inside EVM via pre-compiled contracts, such as on-chain scheduler, bring your own gas, Quality of Service oracle feeds and more. It allows us to add new features that would be impossible on Ethereum while still letting developers use existing code and tooling to create DApps that leverage these features. These pre-compiled smart contracts have static, known addresses on the Acala EVM making it incredibly easy to use.

## Create the Subscription Contract

Let's create our smart contract file by creating a `Subscription.sol` file within the `contracts` folder. In it add the following code:

```text
pragma solidity ^0.6.0;

import "./Scheduler.sol";

contract SubscriptionToken {
  Scheduler constant scheduler = Scheduler(0x0000000000000000000000000000000000000808);
}
```

Here you can see we're importing the abstract class that describes the `Scheduler` smart contract and pointing our `scheduler` to its address. Again, this is a pre-compiled smart contract with a static address. It will always live on-chain at this address, no need to deploy your own version of it.

### Constructor

Next, let's set some state variables and create a constructor. Within the contract add the following:

```text
contract SubscriptionToken {
  contract SubscriptionToken {
    Scheduler constant scheduler = Scheduler(0x0000000000000000000000000000000000000808);

  address payable public owner;
  uint public subscriptionPrice;
  uint public subscriptionPeriod;

  mapping (address => uint) public balanceOf;
  mapping (address => uint) public subTokensOf;
  mapping (address => uint) public periodSubscribed;

  constructor(uint _subscriptionPrice, uint _subscriptionPeriod) public payable {
    owner = msg.sender;
    subscriptionPrice = _subscriptionPrice;
    subscriptionPeriod = _subscriptionPeriod;
  }
}
```

Here we added 6 state variables. Let's go through them one by one:

1. `owner`: The owner of the contract that will receive the subscriptions
2. `subscriptionPrice`: The price to subscribe/re-subscribe
3. `subscriptionPeriod`: The number of blocks before a user is automatically re-subscribed
4. `balanceOf`: The number of native tokens in a user's balance to be used for the re-subscription cost
5. `subTokensOf`: The number of subscription tokens a user has received
6. `periodSubscribed`: The number of `subscriptionPeriod` a user has subscribed for. This will affect how many subscription tokens a user receives upon re-subscribing.

The constructor is self-explanatory, all it does is set the first three state variables for the contract.

### Subscribe/Unsubscribe 

Next let's add the `subscribe`, and `unsubscribe` functions:

```text
contract Subscription {
  ...

  function subscribe() public payable {
    require(periodSubscribed[msg.sender] == 0, "Already subscribed");
    require(msg.value > subscriptionPrice, "Not enough to subscribe");

    uint256 _depositAmount = msg.value - subscriptionPrice;

    balanceOf[msg.sender] = _depositAmount;
    periodSubscribed[msg.sender] = 1;
    subTokensOf[msg.sender] = 1;

    owner.transfer(subscriptionPrice);

    scheduler.scheduleCall(address(this), 0, 50000, 100, 10, abi.encodeWithSignature("pay(address)", msg.sender));
  }

  function unsubscribe() public {
    periodSubscribed[msg.sender] = 0;
  }
}
```

Here, the `subscribe` function ensures the user is depositing sufficient funds into the smart contract to subscribe at least one time, and that the user is not already subscribed. After that, it sets the appropriate values in the contract's state variables.

Finally, we call the scheduler contract:

```text
scheduler.scheduleCall(address(this), 0, 50000, 100, subscriptionPeriod, abi.encodeWithSignature("pay(address)", msg.sender));
```

Let's break this line down a bit:

* `adress(this)` is the address of the smart contract to call, in this case we are calling the same smart contract that is calling the schedule contract
* `subscriptionPeriod` is the number of blocks from now it should try and execute this call \(though it could be more!\)
* `abi.encodeWithSignature("pay(address)"` is the function to call within the smart contract \(we'll define it next\).
* `msg.sender` is the value being passed to that function.

For more information on the rest of the arguments being passed to the `scheduler` contract scroll up to where we created `Scheduler.sol`.

The `unsubscribe` function simply resets the `periodSubscribed` state variable back to 0.

### Payout

Now let's create the `pay` function that the scheduler is calling:

```text
contract Subscription {
  ...

  function pay(address _subscriber) public {
    require(msg.sender == address(this), "No Permission");
    require(periodSubscribed[_subscriber] > 0);

    // If the subscriber does not have enough
    if (balanceOf[_subscriber] < subscriptionPrice) {
      periodSubscribed[_subscriber] = 0;
      return;
    }

    periodSubscribed[_subscriber] += 1;
    balanceOf[_subscriber] -= subscriptionPrice;
    subTokensOf[_subscriber] += periodSubscribed[_subscriber];
    owner.transfer(subscriptionPrice);

    scheduler.scheduleCall(address(this), 0, 50000, 100, subscriptionPeriod, abi.encodeWithSignature("pay(address)", _subscriber));
  }
}
```

Here we ensure the subscriber has enough funds to re-subscribe, and update the various state variables as necessary, giving the subscriber the same number of Subscription Tokens as the number of periods that they have subscribed in a row.

Notice the first line of the function:

```text
require(msg.sender == address(this), "No Permission");
```

What this is saying is that only the contract itself can call this function. When we schedule a call using the `Scheduler` contract, it is not actually the scheduler calling the function but the function itself. The scheduler is just dispatching the call at a later time.

### Add/Withdraw Funds

Finally, let's add a `addFunds`, and `withdrawFunds` function so that the user can add more funds for subscriptions as well as withdraw the unused funds they have sent to the contract.

```text
contract Subscription {
  ...

  function addFunds() public payable {
    balaneOf[msg.sender] += msg.value;
  }

  function wthdrawFunds() public {
    require(balanceOf[msg.sender] > 0, "No funds to withdraw");
    msg.sender.transfer(balanceOf[msg.sender]);
  }
}
```

And that's it! We now have a fully functioning smart contract with automated subscriptions!

## Build

To build the smart contract run the following in the project's root directory:

```text
yarn build
```

## Deploy

To deploy the smart contract follow the directions [here](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/deploy-contracts).

