# Public Detectors

List of public detectors

## State variable shadowing

### Configuration
* Check `shadowing--state`
* Severity: High
* Confidence: High

### What it Detects
Detection of state variables shadowed.

### Exploit Scenario
```solidity
contract BaseContract{
    address owner;

    modifier isOwner(){
        require(owner == msg.sender);
        _;
    }

}

contract DerivedContract is BaseContract{
    address owner;

    constructor(){
        owner = msg.sender;
    }

    function withdraw() isOwner() external{
        msg.sender.transfer(this.balance);
    }
}
```
`owner` of `BaseContract` is never assigned and the modifier `isOwner` does not work.

### Recommendation
Remove the state variable shadowing

## Missing constructor

### Configuration
* Check `missing-constructor`
* Severity: High
* Confidence: Medium

### What it Detects
Detection of a contract that contains sensitives variables without constructor to initiate them.

### Exploit Scenario

```solidity
contract Owner{

    address owner;

    function transferOwnership(address newOwner) external{
        require(owner == msg.sender);
        owner = newOwner;
    }
}
```
`owner` is not initialized at construction. As a result it is always 0 and cannot be changed.

### Recommendation
Initiate the sensitives variables in the constructor.


## Missing return statements

### Configuration
* Check `missing-return`
* Severity: Medium
* Confidence: Medium

### What it Detects
Function with missing return statements

### Exploit Scenario
```solidity
    function f(uint a, uint b) external returns(uint){
        if(a>b){
            return a;
        }
    }
```
`f` has an implicit return statement: if `a<=b`, `f` returns 0 (the default value).


### Recommendation
Explicit all return statements. Initiate all the return variables.

## Dangerous strict equalities

### Configuration
* Check `incorrect-equality`
* Severity: Medium
* Confidence: High

### What it Detects
Use of strick equalities that can be easily manipulated by an attacker.

### Exploit Scenario
```solidity
contract Crowdsale{
    function fund_reached() public returns(bool){
        return this.balance == 100 ether;
    }
```
`Crowdsale` relies on `fund_reached` to know when to stop the sale of tokens. `Crowdsale` reaches 100 ether. Bob sends 0.1 ether. As a result, `fund_reached` is always false and the crowdsale never ends.

### Recommendation
Don't use strict equality to determine if an account has enough ethers or tokens.

## Controlled Lowlevelcall

### Configuration
* Check `controlled-lowlevelcall`
* Severity: Medium
* Confidence: Medium

### What it Detects
Low-level call with a user-controlled `data` field. 

### Exploit Scenario
```solidity
    address token;

    function call_token(bytes data){
        token.call(data);
    }
```
`token` points to an ERC20 token. Bob uses `call_token` to call the `transfer` function of `token` to withdraw all the tokens held by the contract.

### Recommendation
Avoid low-level call. Consider using a whitelist of function ids to call.


## State variable shadowing from abstract contracts

### Configuration
* Check `shadowing--abstract`
* Severity: Medium
* Confidence: High

### What it Detects
Detection of state variables shadowed from abstract contracts.

### Exploit Scenario
```solidity
contract BaseContract{
    address owner;
}

contract DerivedContract is BaseContract{
    address owner;
}
```
`owner` of `BaseContract` is never assigned.

### Recommendation
Remove the state variable shadowing

## Race conditions at contract's initialization

### Configuration
* Check `race-condition-init`
* Severity: High
* Confidence: Medium

### What it Detects
Detect race condition at contract's initialization.

### Exploit Scenario
```solidity
pragma solidity ^0.4.24;

contract Bug{

    bool was_init = false;
    address owner;

    constructor(){}

    function init() not_init{
        was_init = true;
        owner = msg.sender;
    }

    modifier onlyOwner(){
        require(msg.sender == owner);
        _;
    }

    modifier not_init(){
        require(!was_init);
        _;
    }
}
```
Alice deploys `Bug`. Alice calls `init()`. Bob sees Alice's transaction before it has been accepted. Bob calls `init()`. Bob's transaction is accepted before Alice's one. As a result, Bob becomes the owner of the contract.

### Recommendation
Call the initialization function in the contract's constructor.

Upgradable contracts using the delegatecall proxy pattern need an initialization function. Ensure that your deployment script is robust to a race condition during the contract upgrade.

## Unprotected ecrecover leading to a race condition

### Configuration
* Check `race-condition-ecrecover`
* Severity: High
* Confidence: Medium

### What it Detects
Detect incorrect usage of `ecrecover` leading to a race condition.

### Exploit Scenario
```solidity
    mapping(address => bool) valid_signer;

    function bad(bytes code, uint8 recoveryByte, bytes32 ecdsaR, bytes32 ecdsaS){
        bytes32 msghash = keccak256(code);
        
        address signer = ecrecover(msghash, recoveryByte, ecdsaR, ecdsaS);

        require(signer!=0);
        require(valid_signer[signer]);

        msg.sender.transfer(this.balance);
    }
```
Alice calls `bad`. Bob sees Alice's transaction before it has been accepted and calls `bad` with the same parameters. As a result, Bob can withdraw the contract's balance.


### Recommendation
Protect `ecrecover` against race condition. 
In the previous example, you can build `msghash` with the `msg.sender` value:
```solidity
bytes32 msghash = keccak256(msg.sender, code);
```

## Unprotected functions

### Configuration
* Check `unprotected-function`
* Severity: High
* Confidence: Medium

### What it Detects
Detect function that should be protected.

### Exploit Scenario
```solidity
contract Bug{
    address owner = msg.sender;

    function setOwner() external{
        owner = msg.sender;
    }

    modifier isOwner(){
        require(owner == msg.sender);
        _;
    }
}
```
Alice deploys the contract. Bob calls `setOwner` and becomes the owner of the contract.

### Recommendation
Protect functions writing to sensitive state variables.

## Unimplemented functions

### Configuration
* Check `unimplemented`
* Severity: Medium
* Confidence: Medium

### What it Detects
Detect functions with empty body

### Exploit Scenario
```solidity
contract BaseContract{
    function f1() external returns(uint){}
    function f2() external returns(uint){}
}

contract DerivedContract is BaseContract{
    function f1() external returns(uint){
        return 42;
    }
}
```
`DerivedContract` does not overide `f2`. As a result, `f2` will always return 0.

### Recommendation
Do not declare functions with an empty body. Favor [interface](https://solidity.readthedocs.io/en/latest/contracts.html##interfaces) over abstract contract.


## Overflow in ERC20 Balance

### Configuration
* Check `unprotected-erc20-balance`
* Severity: High
* Confidence: Medium

### What it Detects
ERC20 balance is modified without checking for overflow.

### Exploit Scenario
```solidity
    function transfer(address to, uint256 value) external returns (bool){
        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        return true;
    }
```
Alice has 100 tokens. She transfers 101 tokens to Bob. As a result, Alice has an infinite amount of tokens.

### Recommendation
Use `SafeMath`. If `SafeMath` is not used, check manually for overflow.

## Overflow in ERC20 Allowance

### Configuration
* Check `unprotected-erc20-allowance`
* Severity: Medium
* Confidence: Medium

### What it Detects
ERC20 allowance is modified without checking for overflow.

### Exploit Scenario
```solidity
    function buggyDecreaseApproval(address spender, uint value) external{
        allowance[msg.sender][spender] -= value;
    }
```
`buggyDecreaseApproval` decreases the allowance without checking for overflow. 
Alice approves Bob 100 tokens. Alice wants to reduce the allowance of 10 and call `buggyDecreaseApproval` with a `value` of 10. Bob already spent the 100 tokens. As a result, Bob has a large number for the allowance.

### Recommendation
Use `SafeMath`. If `SafeMath` is not used, check manually for overflow.

## Lack of msg.sender usage in transferFrom

### Configuration
* Check `incorrect-transferFrom`
* Severity: Medium
* Confidence: Medium

### What it Detects
`transferFrom` is not using `msg.sender` to decrease the allowance.

### Exploit Scenario
```solidity
    function transferFrom(address from, address to, uint256 value) external returns (bool){
        require(balanceOf[from] >= value);
        require(allowance[from] >= value);

        balanceOf[from] -= value;
        balanceOf[to] -= value;
        return true;
    }
```
`transferFrom` decrease the allowance of `from` instead of the `msg.sender`. As a result, anyone can transfer the tokens to the destination.

### Recommendation
Use `msg.sender` in `transferFrom`

## msg.value usage on non payable functions

### Configuration
* Check `msg-value`
* Severity: Medium
* Confidence: Medium

### What it Detects
If a function uses `msg.value` it should be `payable`. `msg.value` will always be 0 otherwise.

### Exploit Scenario
```solidity
pragma solidity ^0.4.24;

contract NonPayable{

    mapping(address => uint) balances;

    function buy() external{
        balances[msg.sender] += msg.value;
    }

}
```
`buy` is not `payable`. As a result, nobody can buy tokens.

### Recommendation
Add the `payable` attribute to the functions using `msg.value`

## Incorrect erc20 interface

### Configuration
* Check `erc20-interface`
* Severity: Medium
* Confidence: High
### What it Detects
Lack of return value for the ERC20 `approve`/`transfer`/`transferFrom` functions. A contract compiled with solidity > 0.4.22 interacting with these functions will fail to execute them, as the return value is missing.

### Exploit Scenario
```solidity
contract Token{
    function transfer(address to, uint value) external;
    //...
}
```
`Token.transfer` does not return a boolean. Bob deploys the token. Alice creates a contract that interacts with it but assumes a correct ERC20 interface implementation. Alice's contract is unable to interact with Bob's contract.

### Recommendation
Return a boolean for the `approve`/`transfer`/`transferFrom` functions. 


## Deletion on mapping containing a structure

### Configuration
* Check `mapping-deletion`
* Severity: Medium
* Confidence: High

### What it Detects
A deletion on a structure containing a mapping will not delete the mapping (see the [Solidity documentation](https://solidity.readthedocs.io/en/latest/types.html##delete)). The remaining data may be used to compromise the contract.

### Exploit Scenario
```solidity
    struct BalancesStruct{
        address owner;
        mapping(address => uint) balances;
    } 
    mapping(address => BalancesStruct) public stackBalance;

    function remove() internal{
         delete stackBalance[msg.sender];
    }
```
`remove` delete an item of `stackBalance`. The mapping `balances` is never deleted. As a result, `remove` does not work as intended.

### Recommendation
Use of lock-mechanism instead of a deletion to disable structure containing a mapping.

## Variable names too similar

### Configuration
* Check `similar-names`
* Severity: Informational
* Confidence: Medium


### What it Detects
Detect variables with names too similar.

### Exploit Scenario
Bob uses several variables with similar names. As a result, its code is difficult to review.

### Recommendation
Prevent variables to have similar names.
## State variable shadowing

### Configuration
* Check `shadowing--state`
* Severity: High
* Confidence: High

### What it Detects
Detection of state variables shadowed.

### Exploit Scenario
```solidity
contract BaseContract{
    address owner;

    modifier isOwner(){
        require(owner == msg.sender);
        _;
    }

}

contract DerivedContract is BaseContract{
    address owner;

    constructor(){
        owner = msg.sender;
    }

    function withdraw() isOwner() external{
        msg.sender.transfer(this.balance);
    }
}
```
`owner` of `BaseContract` is never assigned and the modifier `isOwner` does not work.

### Recommendation
Remove the state variable shadowing

## Missing constructor

### Configuration
* Check `missing-constructor`
* Severity: High
* Confidence: Medium

### What it Detects
Detection of a contract that contains sensitives variables without constructor to initiate them.

### Exploit Scenario

```solidity
contract Owner{

    address owner;

    function transferOwnership(address newOwner) external{
        require(owner == msg.sender);
        owner = newOwner;
    }
}
```
`owner` is not initialized at construction. As a result it is always 0 and cannot be changed.

### Recommendation
Initiate the sensitives variables in the constructor.


## Missing return statements

### Configuration
* Check `missing-return`
* Severity: Medium
* Confidence: Medium

### What it Detects
Function with missing return statements

### Exploit Scenario
```solidity
    function f(uint a, uint b) external returns(uint){
        if(a>b){
            return a;
        }
    }
```
`f` has an implicit return statement: if `a<=b`, `f` returns 0 (the default value).


### Recommendation
Explicit all return statements. Initiate all the return variables.

## Dangerous strict equalities

### Configuration
* Check `incorrect-equality`
* Severity: Medium
* Confidence: High

### What it Detects
Use of strick equalities that can be easily manipulated by an attacker.

### Exploit Scenario
```solidity
contract Crowdsale{
    function fund_reached() public returns(bool){
        return this.balance == 100 ether;
    }
```
`Crowdsale` relies on `fund_reached` to know when to stop the sale of tokens. `Crowdsale` reaches 100 ether. Bob sends 0.1 ether. As a result, `fund_reached` is always false and the crowdsale never ends.

### Recommendation
Don't use strict equality to determine if an account has enough ethers or tokens.

## Controlled Lowlevelcall

### Configuration
* Check `controlled-lowlevelcall`
* Severity: Medium
* Confidence: Medium

### What it Detects
Low-level call with a user-controlled `data` field. 

### Exploit Scenario
```solidity
    address token;

    function call_token(bytes data){
        token.call(data);
    }
```
`token` points to an ERC20 token. Bob uses `call_token` to call the `transfer` function of `token` to withdraw all the tokens held by the contract.

### Recommendation
Avoid low-level call. Consider using a whitelist of function ids to call.


## State variable shadowing from abstract contracts

### Configuration
* Check `shadowing--abstract`
* Severity: Medium
* Confidence: High

### What it Detects
Detection of state variables shadowed from abstract contracts.

The difference with the `shadowing--state` detector is that the variable is shadowed from an abstract contract, decreasing the issue impact.

### Exploit Scenario
```solidity
contract BaseContract{
    address owner;
}

contract DerivedContract is BaseContract{
    address owner;
}
```
`owner` of `BaseContract` is never assigned.

### Recommendation
Remove the state variable shadowing

## Race conditions at contract's initialization

### Configuration
* Check `race-condition-init`
* Severity: High
* Confidence: Medium

### What it Detects
Detect race condition at contract's initialization.

### Exploit Scenario
```solidity
pragma solidity ^0.4.24;

contract Bug{

    bool was_init = false;
    address owner;

    constructor(){}

    function init() not_init{
        was_init = true;
        owner = msg.sender;
    }

    modifier onlyOwner(){
        require(msg.sender == owner);
        _;
    }

    modifier not_init(){
        require(!was_init);
        _;
    }
}
```
Alice deploys `Bug`. Alice calls `init()`. Bob sees Alice's transaction before it has been accepted. Bob calls `init()`. Bob's transaction is accepted before Alice's one. As a result, Bob becomes the owner of the contract.

### Recommendation
Call the initialization function in the contract's constructor.

Upgradable contracts using the delegatecall proxy pattern need an initialization function. Ensure that your deployment script is robust to a race condition during the contract upgrade.

## Unprotected ecrecover leading to a race condition

### Configuration
* Check `race-condition-ecrecover`
* Severity: High
* Confidence: Medium

### What it Detects
Detect incorrect usage of `ecrecover` leading to a race condition.

### Exploit Scenario
```solidity
    mapping(address => bool) valid_signer;

    function bad(bytes code, uint8 recoveryByte, bytes32 ecdsaR, bytes32 ecdsaS){
        bytes32 msghash = keccak256(code);
        
        address signer = ecrecover(msghash, recoveryByte, ecdsaR, ecdsaS);

        require(signer!=0);
        require(valid_signer[signer]);

        msg.sender.transfer(this.balance);
    }
```
Alice calls `bad`. Bob sees Alice's transaction before it has been accepted and calls `bad` with the same parameters. As a result, Bob can withdraw the contract's balance.


### Recommendation
Protect `ecrecover` against race condition. 
In the previous example, you can build `msghash` with the `msg.sender` value:
```solidity
bytes32 msghash = keccak256(msg.sender, code);
```

## Unprotected functions

### Configuration
* Check `unprotected-function`
* Severity: High
* Confidence: Medium

### What it Detects
Detect function that should be protected.

### Exploit Scenario
```solidity
contract Bug{
    address owner = msg.sender;

    function setOwner() external{
        owner = msg.sender;
    }

    modifier isOwner(){
        require(owner == msg.sender);
        _;
    }
}
```
Alice deploys the contract. Bob calls `setOwner` and becomes the owner of the contract.

### Recommendation
Protect functions writing to sensitive state variables.

## Unimplemented functions

### Configuration
* Check `unimplemented`
* Severity: Medium
* Confidence: Medium

### What it Detects
Detect functions with empty body

### Exploit Scenario
```solidity
contract BaseContract{
    function f1() external returns(uint){}
    function f2() external returns(uint){}
}

contract DerivedContract is BaseContract{
    function f1() external returns(uint){
        return 42;
    }
}
```
`DerivedContract` does not overide `f2`. As a result, `f2` will always return 0.

### Recommendation
Do not declare functions with an empty body. Favor [interface](https://solidity.readthedocs.io/en/latest/contracts.html##interfaces) over abstract contract.


## Overflow in ERC20 Balance

### Configuration
* Check `unprotected-erc20-balance`
* Severity: High
* Confidence: Medium

### What it Detects
ERC20 balance is modified without checking for overflow.

### Exploit Scenario
```solidity
    function transfer(address to, uint256 value) external returns (bool){
        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        return true;
    }
```
Alice has 100 tokens. She transfers 101 tokens to Bob. As a result, Alice has an infinite amount of tokens.

### Recommendation
Use `SafeMath`. If `SafeMath` is not used, check manually for overflow.

## Overflow in ERC20 Allowance

### Configuration
* Check `unprotected-erc20-allowance`
* Severity: Medium
* Confidence: Medium

### What it Detects
ERC20 allowance is modified without checking for overflow.

### Exploit Scenario
```solidity
    function buggyDecreaseApproval(address spender, uint value) external{
        allowance[msg.sender][spender] -= value;
    }
```
`buggyDecreaseApproval` decreases the allowance without checking for overflow. 
Alice approves Bob 100 tokens. Alice wants to reduce the allowance of 10 and call `buggyDecreaseApproval` with a `value` of 10. Bob already spent the 100 tokens. As a result, Bob has a large number for the allowance.

### Recommendation
Use `SafeMath`. If `SafeMath` is not used, check manually for overflow.

## Lack of msg.sender usage in transferFrom

### Configuration
* Check `incorrect-transferFrom`
* Severity: Medium
* Confidence: Medium

### What it Detects
`transferFrom` is not using `msg.sender` to decrease the allowance.

### Exploit Scenario
```solidity
    function transferFrom(address from, address to, uint256 value) external returns (bool){
        require(balanceOf[from] >= value);
        require(allowance[from] >= value);

        balanceOf[from] -= value;
        balanceOf[to] -= value;
        return true;
    }
```
`transferFrom` decrease the allowance of `from` instead of the `msg.sender`. As a result, anyone can transfer the tokens to the destination.

### Recommendation
Use `msg.sender` in `transferFrom`

## msg.value usage on non payable functions

### Configuration
* Check `msg-value`
* Severity: Medium
* Confidence: Medium

### What it Detects
If a function uses `msg.value` it should be `payable`. `msg.value` will always be 0 otherwise.

### Exploit Scenario
```solidity
pragma solidity ^0.4.24;

contract NonPayable{

    mapping(address => uint) balances;

    function buy() external{
        balances[msg.sender] += msg.value;
    }

}
```
`buy` is not `payable`. As a result, nobody can buy tokens.

### Recommendation
Add the `payable` attribute to the functions using `msg.value`

## Incorrect erc20 interface

### Configuration
* Check `erc20-interface`
* Severity: Medium
* Confidence: High
### What it Detects
Lack of return value for the ERC20 `approve`/`transfer`/`transferFrom` functions. A contract compiled with solidity > 0.4.22 interacting with these functions will fail to execute them, as the return value is missing.

### Exploit Scenario
```solidity
contract Token{
    function transfer(address to, uint value) external;
    //...
}
```
`Token.transfer` does not return a boolean. Bob deploys the token. Alice creates a contract that interacts with it but assumes a correct ERC20 interface implementation. Alice's contract is unable to interact with Bob's contract.

### Recommendation
Return a boolean for the `approve`/`transfer`/`transferFrom` functions. 


## Deletion on mapping containing a structure

### Configuration
* Check `mapping-deletion`
* Severity: Medium
* Confidence: High

### What it Detects
A deletion on a structure containing a mapping will not delete the mapping (see the [Solidity documentation](https://solidity.readthedocs.io/en/latest/types.html##delete)). The remaining data may be used to compromise the contract.

### Exploit Scenario
```solidity
    struct BalancesStruct{
        address owner;
        mapping(address => uint) balances;
    } 
    mapping(address => BalancesStruct) public stackBalance;

    function remove() internal{
         delete stackBalance[msg.sender];
    }
```
`remove` delete an item of `stackBalance`. The mapping `balances` is never deleted. As a result, `remove` does not work as intended.

### Recommendation
Use of lock-mechanism instead of a deletion to disable structure containing a mapping.

## Variable names too similar

### Configuration
* Check `similar-names`
* Severity: Informational
* Confidence: Medium


### What it Detects
Detect variables with names too similar.

### Exploit Scenario
Bob uses several variables with similar names. As a result, its code is difficult to review.

### Recommendation
Prevent variables to have similar names.