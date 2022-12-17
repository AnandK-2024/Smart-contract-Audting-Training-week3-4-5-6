# Function Default Visibility


The function visibility feature in Solidity smart contracts is used to ensure that when functions are specified, their level of accessibility, including public, external, internal, and private, are maintained as they were intended by the developers.

## Who can call a smart contract function?

There are three kinds of contracts that can call a function: 

* the main contract itself

* contract inherited from the main contract (DerivedContract)

* third-party contract (OutsideContract).

## 1. Public

A public function can be accessed by any of the three types of calling contracts: main contract, derived contract, and a third party contract. A function is public by default.

## 2. External

An external function is a function that can only be called by a third party. With the external function visibility, a contract that can call the function must be independent of the main contract and can not be a derived contract.


## 3. Internal
An internal function can be called by the main contract and any of its derived contracts. Internal functions are accessible from the main contract in which they were initially declared and by the contracts that extend from this main contract through inheritance. 

## 4. Private

A private function can only be called by the main contract in which it was specified. Private functions are used initially according to common practice, but if the scope is wider than this modifier type, any other plausible modifier should be used.

#### Main contract
![Screenshot (86)](https://user-images.githubusercontent.com/82324643/208231320-c885491a-5dc9-401a-8841-b8a8425a1df4.png)

#### Attacker contract
![Screenshot (87)](https://user-images.githubusercontent.com/82324643/208231325-0627e71f-e877-4a60-9c0c-a12ff8924a14.png)







## function visibility modifiers help gas optimization

Because the parameters for external function visibility modifier are not saved to memory, but read directly from calldata, smart contracts consume less gas. In contrast public functions use input parameters are saved to memory, which costs more gas to deploy a smart contract.
