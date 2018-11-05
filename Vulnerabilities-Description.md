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


## Contracts that lock ether	
* Check: `locked-ether	`
* Severity: Medium
* Confidence: Medium

### Description
Contract with a `payable` function, but without a withdraw capacity.

### Exploit scenario
```solidity
pragma solidity 0.4.24;
contract Locked{
    function receive() payable public{
    }
}
```
Every ethers send to `Locked` will be lost.

### Recommendation
Remove the payable attribute or add a withdraw function.

## Dangerous usage of `tx.origin`	
* Check: `tx-origin`
* Severity: Medium
* Confidence: Medium

### Description
`tx.origin`-based protection can be abused by malicious contract if a legitimate user interacts with the malicious contract.

### Exploit scenario
```solidity
contract TxOrigin {
    address owner = msg.sender;

    function bug() {
        require(tx.origin == owner);
    }
```
Bob is the owner of `TxOrigin`. Bob calls Eve's contract. Eve's contact calls `TxOrigin` and bypass the `tx.origin` protection.

### Recommendation
Do not use `tx.origin` for authentification.

## Assembly usage		
* Check: `assembly`
* Severity: Informational
* Confidence: High

### Description
The use of assembly is error-prone and should be avoided.

### Recommendation
Do not use evm assembly.

## State variables that could be declared constant			
* Check: `constable-states`
* Severity: Informational
* Confidence: High

### Description
Constant state variable should be declared constant to save gas.

### Recommendation
Add the `constant` attributes to the state variables that never change.

## Public function that could be declared as external
* Check: `external-function	`
* Severity: Informational
* Confidence: High

### Description
`public` functions that are never called by the contract should be declared `external` to save gas.

### Recommendation
Use the `external` attribute for functions never called from the contract.

## Public function that could be declared as external
* Check: `external-function	`
* Severity: Informational
* Confidence: High

### Description
`public` functions that are never called by the contract should be declared `external` to save gas.

### Recommendation
Use the `external` attribute for functions never called from the contract.











