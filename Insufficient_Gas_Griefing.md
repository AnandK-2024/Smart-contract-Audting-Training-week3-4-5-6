# Insufficient Gas Griefing

This attack can be done on contracts which accept data and use it in a sub-call on another contract. This method is often used in multisignature wallets as well as transaction relayers. If the sub-call fails, either the whole transaction is reverted, or execution is continued.

Let's consider a simple relayer contract as an example. As shown below, the relayer contract allows someone to make and sign a transaction, without having to execute the transaction. Often this is used when a user can't pay for the gas associated with the transaction.



![Screenshot (102)](https://user-images.githubusercontent.com/82324643/208285587-4ad6b93f-0508-4ea8-ab6e-83401098073c.png)


The user who executes the transaction, the 'forwarder', can effectively censor transactions by using just enough gas so that the transaction executes, but not enough gas for the sub-call to succeed.

## Prevent technique

There are two ways this could be prevented.
1.The first solution would be to only allow trusted users to relay transactions. 

2.The other solution is to require that the forwarder provides enough gas, as seen below.

    // contract called by Relayer
    contract Executor {
        function execute(bytes _data, uint _gasLimit) {
            require(gasleft() >= _gasLimit);
            ...
        }
    }
