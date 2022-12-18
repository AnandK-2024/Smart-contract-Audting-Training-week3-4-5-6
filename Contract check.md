# Avoid using extcodesize to check for externally owned accounts(EOA)

Checking if a call was made from an Externally Owned Account (EOA) or a contract account is typically done using `extcodesize` check which may be circumvented by a contract during construction when it does not have source code available.




    contract OnlyForEOA {    
        uint public flag;

            // The following modifier (or a similar check) is often used to verify
            //whether a call was made from an externally owned account (EOA) or a contract account:
        // only EOA can pass this function
        modifier isNotContract(address _a){
            uint len;
            assembly { len := extcodesize(_a) }
            require(len == 0);
            _;
        }

        // only for EOA(can call this function)  
        function setFlag(uint i) public isNotContract(msg.sender){
            flag = i;
        }
    }

The idea is straight forward: if an address contains code, it’s not an EOA but a contract account. However, a contract does not have source code available during construction. This means that while the constructor is running, it can make calls to other contracts, 
but extcodesize for its address returns zero. 

      contract Attacker {
          constructor(address _a) public {
              OnlyForEOA c = OnlyForEOA(_a);
              c.setFlag(1);
          }
      }
      
      
      
### prevent technique

check the value of `(tx.origin == msg.sender)` rather than using `excodesize`.
