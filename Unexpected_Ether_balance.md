# Unexpected Ether balance

Contracts can behave erroneously when they strictly assume a specific Ether balance. It is always possible to forcibly send ether to a contract (without triggering its fallback function), using `selfdestruct`, or by `mining to the account`. Never use conditions or contracts that assume that the contract balance is zero. 


## Selfdestruct

When the `SELFDESTRUCT` opcode is called, funds of the `calling address` are sent to the `address on the stack`, and execution is immediately `halted`. Since this opcode works on the EVM-level, Solidity-level functions that might block the receipt of Ether will not be executed.


This means that if a contract’s function has a conditional statement that depends on that contract’s balance being below a certain amount, that statement can be potentially bypassed:


![Screenshot (99)](https://user-images.githubusercontent.com/82324643/208237959-4e0ee897-000e-4d84-a593-da6daa73b17c.png)

Due to the throwing `fallback function`, normally the contract cannot `receive ether`. However, if a contract `selfdestructs` with this contract as a `target`, the fallback function does not get called. As a result `address(this).balance` becomes greater than 0, and thus the attacker can bypass the require statement in `onlyNonZeroBalance`.
## Pre-calculated Deployments

## Block Rewards and Coinbase
