# Missing Protection against Signature Replay Attacks

It is sometimes necessary to perform signature verification in smart contracts to achieve better usability or to save gas cost. 

transferProxy() (Proxy Transfer) is a newly proposed EIP where the users can pay transaction fees in tokens, as opposed to paying only in ether in tradition ERC20 contracts. Proxy Transfer aims to solve the problem where the users can’t freely transfer their tokens unless they hold enough ether to pay for the transaction fees. 

### Attack Workflow

![image](https://user-images.githubusercontent.com/82324643/208288019-dfff0a1b-07de-413b-9857-59333fb3b849.png)

① Alice (sender) initiates a transaction in which 100 Token 1s will be sent to Bob (recipient) and 3 Token 1s will be paid to Proxy as service fee. Alice then signs the transaction with her signature → sig(A,B,100,3).

② Transaction gets carried out by Proxy. Bob gets 100 Token 1s from Alice.

③ Bob replays Alice’s signature in a new transaction → transferProxy(A,B,100,3,sig) → sig(A,B,100,3).

④ New transaction gets carried out by Proxy. Bob gets 100 Token 2s from Alice without her authorization.



    function transferProxy(address _from, address _to, uint256 _value, uint256 _fee,
        uint8 _v, bytes32 _r, bytes32 _s) public returns (bool){

        if(balances[_from] < _fee + _value || _fee > _fee + _value) revert();

        uint256 nonce = nonces[_from];        // nonce of sender(signe)
        bytes32 h = keccak256(_from,_to,_value,_fee,nonce);         // message hash
        if(_from != ecrecover(h,_v,_r,_s)) revert();                 // signer verification

        if(balances[_to] + _value < balances[_to]
            || balances[msg.sender] + _fee < balances[msg.sender]) revert();
        balances[_to] += _value;
        emit Transfer(_from, _to, _value);

        balances[msg.sender] += _fee;
        emit Transfer(_from, msg.sender, _fee);

        balances[_from] -= _value + _fee;
        nonces[_from] = nonce + 1;                                // increase nonce
        return true;
    }
    
    
    
    
### How to Prevent Such Attacks

There certainly are many possible ways to prevent such replay attacks, here are some of my ideas:

* use timestamp 
* Make nonce start at a higher count than 0
* Add chainID, name of the public chain in smart contracts
* Add address(this) in keccak256()




      bytes32 private constant _PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");

      function permit(
              address owner,
              address spender,
              uint256 value,
              uint256 deadline,
              uint8 v,
              bytes32 r,
              bytes32 s
          ) public virtual override {
              require(block.timestamp <= deadline,  "expired deadline");
               if(balances[_from] < _fee + _value || _fee > _fee + _value) revert();
               uint256 nonce = nonces[_from]; 

              bytes32 structHash = keccak256(abi.encode(_PERMIT_TYPEHASH, owner, spender, value, _useNonce(owner), deadline));

              bytes32 hash = _hashTypedDataV4(structHash);

              address signer = ECDSA.recover(hash, v, r, s);
              require(signer == owner, "ECDSA invalid signature");

              _approve(owner, spender, value);
          }
          
          
for more info reach [out](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/ERC20Permit.sol)
