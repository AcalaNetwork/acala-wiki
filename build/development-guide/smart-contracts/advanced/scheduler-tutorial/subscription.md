# Writing the Smart Contract

The smart contract we're writing is a basic automatic subscription. The user can deposit funds that will automatically be used up after a set number of blocks. Everytime the user "re-subscribes", they are given some tokens. The more times a user re-subscribes, the more tokens they are given every time they re-subscribe.

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

The Acala EVM integrates it's unique on-chain features to the EVM by using pre-compiled smart contracts that interact with Acala on the chain level. This allows us add new features that would be impossible on Ethereum while still letting developers use existing code and tooling to create dapps that leverage these features. These pre-compiled smart contracts have static, known addresses on the Acala EVM making it incredibly easy to use them in your code.

## Creating a New Contract

Let's create our smart contract file by creating a `Subscription.sol` file within our `contracts` folder. In it add the following code:

```text
pragma solidity ^0.6.0;

import "./Scheduler.sol";

contract SubscriptionToken {
  Scheduler constant scheduler = Scheduler(0x0000000000000000000000000000000000000808);
}
```

Here you can see we're importing the abstract class that describes the `Scheduler` smart contract and pointing our `scheduler` to it's address. Again, this is a pre-compiled smart contract with a static address. It will always live on chain at this address, no need to deploy your own version of it.

Next lets set some state variables and create a constructor. Within the contract add the following:

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

The constructor is self explanatory, all it does is set the first three state variables for the contract.

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

Here, the `subscribe` function ensures the user is depositing sufficient funds into the smart contract to subscribe at least one time, and that the user is not already subscribed. After that it sets the appropriate values in the contract's state variables.

Finally, we call the scheduler contract:

```text
scheduler.scheduleCall(address(this), 0, 50000, 100, subscriptionPeriod, abi.encodeWithSignature("pay(address)", msg.sender));
```

Let's break this line down a bit:

- `adress(this)` is the address of the smart contract to call, in this case we are calling the same smart contract that is calling the schedule contract
- `subscriptionPeriod` is the number of blocks from now it should try and execute this call (though it could be more!)
- `abi.encodeWithSignature("pay(address)"` is the function to call within the smart contract (we'll define it next).
- `msg.sender` is the value being passed to that function.

For more information on the rest of the arguments being passed to the `scheduler` contract scroll up to where we created `Scheduler.sol`.

The `unsubscribe` function simply resets the `periodSubscribed` state variable back to 0.

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

Finally let's add a `addFunds`, and `withdrawFunds` function so that the user can add more funds for subscriptions as well as withdraw the unused funds they have sent to the contract.

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

## Building

To build the smart contract run the following in the project's root directory:

```text
yarn build
```

## Deploying

To deploy the smart contract follow the directions [here](https://wiki.acala.network/build/development-guide/smart-contracts/get-started-evm/deploy-contracts).
