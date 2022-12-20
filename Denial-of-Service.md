
# Denial-of-Service




## 1. DoS With Block Gas Limit

When smart contracts are deployed or functions inside them are called, the execution of these actions always requires a certain amount of gas, based of how much computation is needed to complete them. The Ethereum network specifies a block gas limit and the sum of all transactions included in a block can not exceed the threshold.

Programming patterns that are harmless in centralized applications can lead to Denial of Service conditions in smart contracts when the cost of executing a function exceeds the block gas limit. Modifying an array of unknown size, that increases in size over time, can lead to such a Denial of Service condition.

![Screenshot (93)](https://user-images.githubusercontent.com/82324643/208234739-76586ebc-f254-46ea-8d77-1742c1301e59.png)


An attacker can create many user accounts making the investor array large. In principle this can be done such that the gas required to execute the for loop exceeds the block gas limit, making the `distribute()` function essentially unusable.

## 2. DoS with Failed Call



![Screenshot (94)](https://user-images.githubusercontent.com/82324643/208235165-a50e5b01-46dd-4459-9e22-e16841f4b0ae.png)


If attacker bids using a smart contract which has a fallback function that reverts any payment, the attacker can win any auction. When it tries to refund the old leader, it reverts if the refund fails. This means that a malicious bidder can become the leader while making sure that any refunds to their address will always fail. In this way, they can prevent anyone else from calling the `bid()` function, and stay the leader forever. A recommendation is to set up a pull payment system instead, as described earlier.


### condition for transaction via `send()`

![image](https://user-images.githubusercontent.com/82324643/208234482-823f019f-1488-49c9-b1e3-b24acb161595.png)


## 3. Gas Limit DoS on the Network via Block Stuffing


#### Ethernaut contract

In this `contract` external call sending 1% of the contract balance to a user-specified account. The reason the` CALL opcode` is used, is to ensure that the owner still gets paid, even if the external call reverts. The issue is that the transaction will send all of its gas (in reality, only most of the transaction gas is sent, some is left to finish processing the call) to the external call. If the user were malicious they could create a contract that would consume all the gas, and force all transactions to `withdraw()` to fail, due to running out of gas.


![Screenshot (95)](https://user-images.githubusercontent.com/82324643/208235435-6c2aa446-910d-4a9c-9247-e86e105dc8af.png)


#### Attacker contract

![Screenshot (97)](https://user-images.githubusercontent.com/82324643/208235580-25bee4d4-e05f-4d9d-8c75-7903491dfd85.png)

#### Prevent technique

To prevent such DOS attack vectors, ensure a gas stipend is specified in an external call, to limit the amount of gas that  transaction can use.


### Assert Violation

#### `assert()` uses the `0xfe` opcode to cause an error condition

If you look up `0xfe` `opcodes` in the `yellow paper`, you won’t find them. This is why you see the `invalid opcode error`, because there’s no specification for how a client should handle them.

`assert()` is just there to prevent anything really bad from happening, but it shouldn’t be possible for the condition to evaluate to false.

