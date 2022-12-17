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
### Suppression

