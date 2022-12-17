#  Re-Entrancy

External contracts can hijack the control flow, which is one of the main risks of using them. Reentrancy attacks, also known as recursive call attacks, occur when a malicious contract calls into the calling contract before the function's initial invocation has finished. This could lead to undesirable interactions between the various invocations of the function.

## The vulnerability

When a contract sends ether to an unidentified address, this attack may take place. Attackers can construct a contract with malicious code in the fallback function and send it to an external address. Therefore, the malicious code will be executed whenever a contract sends ether to this address. Typically, the malicious code runs a function on the weak contract and carries out actions that the developer did not intend.


#### Main contract


![Screenshot (88)](https://user-images.githubusercontent.com/82324643/208232269-76c0069a-a158-4b97-ae02-dd37561fcc23.png)

This contract has two public functions. `donate()` and `withdraw()`. The donate() function simply increments the senders balances. The `withdraw()` function allows the sender to specify the amount of wei to withdraw.


#### Attacker contract

![Screenshot (89)](https://user-images.githubusercontent.com/82324643/208233305-064570b1-ee72-4b1d-a2ef-951332c2ba74.png)


## Preventative Techniques

* Ensure that all logic that changes state variables happen before ether is sent out of the contract (or any external call).

![Screenshot (90)](https://user-images.githubusercontent.com/82324643/208233442-09784259-2ad6-4796-b4e9-82246c39bb15.png)

* using openzaplien Re-entracy guard


![Screenshot (92)](https://user-images.githubusercontent.com/82324643/208233925-7c11a295-1aba-4c42-bbee-c7d4ed408520.png)



