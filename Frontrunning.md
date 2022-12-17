# Frontrunning

Since all transactions are visible in the mempool for a short while before being executed, observers of the network can see and react to an action before it is included in a block. An example of how this can be exploited is with a decentralized exchange where a buy order transaction can be seen, and second order can be broadcast and executed before the first transaction is included.

## The following categories of front-running attacks:

### Displacement

it is not important for Alice’s (User) function call to run after Mallory (Adversary) runs her function. 

Examples of displacement include:

* Alice trying to register a domain name and Mallory registering it first;
* Alice trying to submit a bug to receive a bounty and Mallory stealing it and submitting it first;
* Alice trying to submit a bid in an auction and Mallory copying it.

This attack is commonly performed by increasing the `gasPrice` higher than network average, often by a multiplier of 10 or more.

### Insertion

The adversary needs the original function call to execute after her transaction in order to successfully carry out this kind of attack.

Mallory needs Alice's original function to execute on the modified state of the contract after she runs her function in an insertion attack.

Mallory will insert two transactions: 

1. buying the asset at the best offer price and 
2. then selling it to Alice at her slightly higher purchase price, for instance. 

if Alice places a purchase order on a blockchain asset at a price higher than the best offer. Mallory will profit from the price difference without having to hold the asset if Alice's transaction is then followed up on.



## ERC20 API: An Attack Vector on the Approve/TransferFrom Methods


    function approve(address _ spender, uint256 _ value) returns (bool success)

#### Attack Scenario

Here is a possible attack scenario:

* Alice allows Bob to transfer N of Alice's tokens (N>0)  by calling the approve method on a Token smart contract, passing the Bob's address and N as the method arguments
* After some time, Alice decides to change from N to M (M>0) the number of Alice's tokens Bob is allowed to transfer, so she calls the approve method again, this time passing the Bob's address and M as the method arguments
* Bob notices the Alice's second transaction before it was mined and quickly sends another transaction that calls the transferFrom method to transfer N Alice's tokens somewhere
* If the Bob's transaction will be executed before the Alice's transaction, then Bob will successfully transfer N Alice's tokens and will gain an ability to transfer another M tokens
* Before Alice noticed that something went wrong, Bob calls the transferFrom method again, this time to transfer M Alice's tokens.
So, an Alice's attempt to change the Bob's allowance from N to M (N>0 and M>0) made it possible for Bob to transfer N+M of Alice's tokens, while Alice never wanted to allow so many of her tokens to be transferred by Bob.
