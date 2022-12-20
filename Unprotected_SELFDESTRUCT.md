
# Unprotected Selfdestruct Instruction
In contracts that have a `selfdestruct` method, if there are missing or insufficient access controls, malicious actors can `self-destruct the contract`. It's important to consider whether self-destruct functionality is absolutely necessary. If it is necessary, consider using a multisig authorization to prevent an attack.

This attack was used in the Parity Attack. An anonymous user located, and exploited a vulnerability in the "library" smart contract, making themself the `contract owner`. The attacker then proceeded to self-destruct the contract. This led to funds being blocked in 587 unique wallets, holding a total of 513,774.16 Ether.


If a `selfdestruct` function is unprotected, i.e. it has missing access controls; a malicious user could destroy the contract and possibly drain all the funds to their account if they can control the address input as well.

#### Example:
We can notice the `selfDestruct` function is public and hence any user can call the function and supply their `address to withdraw all the funds from the contract` and destroyed the contract.

    function selfDestruct(address adr) public {                  
        selfdestruct(payable(adr));
    }
