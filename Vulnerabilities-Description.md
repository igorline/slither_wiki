## Suicidal
* Check: `suicidal`
* Severity: High
* Confidence: High

### Description
Unprotected call to a function executing `selfdestruct`/`suicide` 

### Exploit Scenario
```solidity
contract Suicidal{
    function kill() public{
        selfdestruct(msg.value);
    }
}
```
Bob calls `kill` and destruct the contract.

### Recommendation
Protect the access to all sensitive functions.



## Uninitialized state variables	
* Check: `uninitialized-state`
* Severity: High
* Confidence: High

### Description
Uninitialized state variables.

### Exploit Scenario
```solidity
contract Uninitialized{
    address destination;

    function transfer() payable public{
        destination.transfer(msg.value);
    }
}
```
Bob calls `transfer`. As a result, the ethers are sent to the address 0x0 and are lost.

### Recommendation
Initialize all the variables. If a variable is meant to be initialized to zero, explicitly set it to zero.


## Uninitialized storage variables	
* Check: `uninitialized-storage`
* Severity: High
* Confidence: High

### Description
An uinitialized storage variable will act as a reference to the first state variable, and can override a critical variable.

### Exploit Scenario
```solidity
contract Uninitialized{
    address owner = msg.sender;

    struct St{
        uint a;
    }

    function func() {
        St st;
        st.a = 0x0;
    }    
}
```
Bob calls `func`. As a result, `owner` is override to 0.

### Recommendation
Initialize all the storage variables.

## Functions that send ether to arbitrary destinations	
* Check: `arbitrary-send`
* Severity: High
* Confidence: Medium

### Description
Unprotected call to a function executing sending ethers to an arbitrary address.

### Exploit Scenario
```solidity
contract ArbitrarySend{
    address destination;
    function setDestination(){
        destination = msg.sender; 
    }

    function withdraw() public{
        destination.transfer(this.balance);
    }
}
```
Bob calls `setDestination` and `withdraw`. As a result he withdraws the contract's balance.

### Recommendation
Ensure that an arbitrary user cannot withdraw unauthorize funds.


## Reentrancy vulnerabilities	
* Check: `reentrancy`
* Severity: High
* Confidence: Medium

### Description
Detection of the [re-entrancy bug](https://github.com/trailofbits/not-so-smart-contracts/tree/master/reentrancy).

### Exploit scenario
```solidity
    function withdrawBalance(){
        // send userBalance[msg.sender] ethers to msg.sender
        // if mgs.sender is a contract, it will call its fallback function
        if( ! (msg.sender.call.value(userBalance[msg.sender])() ) ){
            throw;
        }
        userBalance[msg.sender] = 0;
    }   
```

Bob uses the re-entrancy bug to call `withdrawBalance` two times, and withdraw more than its initial deposit to the contract.

### Recommendation
Apply the [check-effects-interactions pattern](http://solidity.readthedocs.io/en/v0.4.21/security-considerations.html#re-entrancy).