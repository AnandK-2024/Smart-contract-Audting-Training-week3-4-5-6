# Unexpected Ether balance

Contracts can behave erroneously when they strictly assume a specific Ether balance. It is always possible to forcibly send ether to a contract (without triggering its fallback function), using `selfdestruct`, or by `mining to the account`. Never use conditions or contracts that assume that the contract balance is zero. 


## 1. Selfdestruct

When the `SELFDESTRUCT` opcode is called, funds of the `calling address` are sent to the `address on the stack`, and execution is immediately `halted`. Since this opcode works on the EVM-level, Solidity-level functions that might block the receipt of Ether will not be executed.


This means that if a contract’s function has a conditional statement that depends on that contract’s balance being below a certain amount, that statement can be potentially bypassed:


![Screenshot (99)](https://user-images.githubusercontent.com/82324643/208237959-4e0ee897-000e-4d84-a593-da6daa73b17c.png)

Due to the throwing `fallback function`, normally the contract cannot `receive ether`. However, if a contract `selfdestructs` with this contract as a `target`, the fallback function does not get called. As a result `address(this).balance` becomes greater than 0, and thus the attacker can bypass the require statement in `onlyNonZeroBalance`.
## 2. Pre-calculated Deployments

Additionally, the target address of newly deployed smart contracts is generated in a deterministic fashion. The address generation can be looked up in any EVM implementation, such as the py-evm reference implementation by the Ethereum Foundation:

      def generate_contract_address(address: Address, nonce: int) -> Address:
          return force_bytes_to_address(keccak(rlp.encode([address, nonce])))
          
for more details reach [out](https://github.com/ethereum/py-evm/blob/e924f63992a35212616b4e20355d161bc4348925/eth/_utils/address.py#L17-L18)

An attacker can send funds to this address before the deployment has happened.

## 3. Block Rewards and Coinbase

Depending on the attacker's capabilities, they can also start proof-of-work mining. By setting the target address to their coinbase, block rewards will be added to its balance.

for more details reach [out](https://consensys.github.io/smart-contract-best-practices/attacks/force-feeding/#selfdestruct)
