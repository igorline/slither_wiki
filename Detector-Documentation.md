# Public Detectors

List of public detectors

Num | Detector | What it Detects | Impact | Confidence
--- | --- | --- | --- | ---
1 | `rtlo` | [Right-To-Left-Override control character is used](https://github.com/crytic/slither/wiki/Detector-Documentation#right-to-left-override-character) | High | High
2 | `shadowing-state` | [State variables shadowing](https://github.com/crytic/slither/wiki/Detector-Documentation#state-variable-shadowing) | High | High
3 | `suicidal` | [Functions allowing anyone to destruct the contract](https://github.com/crytic/slither/wiki/Detector-Documentation#suicidal) | High | High
4 | `uninitialized-state` | [Uninitialized state variables](https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-state-variables) | High | High
5 | `uninitialized-storage` | [Uninitialized storage variables](https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-storage-variables) | High | High
6 | `arbitrary-send` | [Functions that send ether to arbitrary destinations](https://github.com/crytic/slither/wiki/Detector-Documentation#functions-that-send-ether-to-arbitrary-destinations) | High | Medium
7 | `controlled-delegatecall` | [Controlled delegatecall destination](https://github.com/crytic/slither/wiki/Detector-Documentation#controlled-delegatecall) | High | Medium
8 | `reentrancy-eth` | [Reentrancy vulnerabilities (theft of ethers)](https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities) | High | Medium
9 | `erc20-interface` | [Incorrect ERC20 interfaces](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-erc20-interface) | Medium | High
10 | `erc721-interface` | [Incorrect ERC721 interfaces](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-erc721-interface) | Medium | High
11 | `incorrect-equality` | [Dangerous strict equalities](https://github.com/crytic/slither/wiki/Detector-Documentation#dangerous-strict-equalities) | Medium | High
12 | `locked-ether` | [Contracts that lock ether](https://github.com/crytic/slither/wiki/Detector-Documentation#contracts-that-lock-ether) | Medium | High
13 | `shadowing-abstract` | [State variables shadowing from abstract contracts](https://github.com/crytic/slither/wiki/Detector-Documentation#state-variable-shadowing-from-abstract-contracts) | Medium | High
14 | `constant-function` | [Constant functions changing the state](https://github.com/crytic/slither/wiki/Detector-Documentation#constant-functions-changing-the-state) | Medium | Medium
15 | `reentrancy-no-eth` | [Reentrancy vulnerabilities (no theft of ethers)](https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-1) | Medium | Medium
16 | `tx-origin` | [Dangerous usage of `tx.origin`](https://github.com/crytic/slither/wiki/Detector-Documentation#dangerous-usage-of-txorigin) | Medium | Medium
17 | `unchecked-lowlevel` | [Unchecked low-level calls](https://github.com/crytic/slither/wiki/Detector-Documentation#unchecked-low-level-calls) | Medium | Medium
18 | `unchecked-send` | [Unchecked send](https://github.com/crytic/slither/wiki/Detector-Documentation#unchecked-send) | Medium | Medium
19 | `uninitialized-local` | [Uninitialized local variables](https://github.com/crytic/slither/wiki/Detector-Documentation#uninitialized-local-variables) | Medium | Medium
20 | `unused-return` | [Unused return values](https://github.com/crytic/slither/wiki/Detector-Documentation#unused-return) | Medium | Medium
21 | `shadowing-builtin` | [Built-in symbol shadowing](https://github.com/crytic/slither/wiki/Detector-Documentation#builtin-symbol-shadowing) | Low | High
22 | `shadowing-local` | [Local variables shadowing](https://github.com/crytic/slither/wiki/Detector-Documentation#local-variable-shadowing) | Low | High
23 | `void-cst` | [Constructor called not implemented](https://github.com/crytic/slither/wiki/Detector-Documentation#void-constructor) | Low | High
24 | `calls-loop` | [Multiple calls in a loop](https://github.com/crytic/slither/wiki/Detector-Documentation/_edit#calls-inside-a-loop) | Low | Medium
25 | `reentrancy-benign` | [Benign reentrancy vulnerabilities](https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-2) | Low | Medium
26 | `timestamp` | [Dangerous usage of `block.timestamp`](https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp) | Low | Medium
27 | `assembly` | [Assembly usage](https://github.com/crytic/slither/wiki/Detector-Documentation#assembly-usage) | Informational | High
28 | `deprecated-standards` | [Deprecated Solidity Standards](https://github.com/crytic/slither/wiki/Detector-Documentation#deprecated-standards) | Informational | High
29 | `erc20-indexed` | [Un-indexed ERC20 event parameters](https://github.com/crytic/slither/wiki/Detector-Documentation#unindexed-erc20-event-parameters) | Informational | High
30 | `low-level-calls` | [Low level calls](https://github.com/crytic/slither/wiki/Detector-Documentation#low-level-calls) | Informational | High
31 | `naming-convention` | [Conformance to Solidity naming conventions](https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions) | Informational | High
32 | `pragma` | [If different pragma directives are used](https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used) | Informational | High
33 | `solc-version` | [Incorrect Solidity version (< 0.4.24 or complex pragma)](https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity) | Informational | High
34 | `unused-state` | [Unused state variables](https://github.com/crytic/slither/wiki/Detector-Documentation#unused-state-variables) | Informational | High
35 | `too-many-digits` | [Conformance to numeric notation best practices](https://github.com/crytic/slither/wiki/Detector-Documentation#too-many-digits) | Informational | Medium
36 | `constable-states` | [State variables that could be declared constant](https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-constant) | Optimization | High
37 | `external-function` | [Public function that could be declared as external](https://github.com/crytic/slither/wiki/Detector-Documentation#public-function-that-could-be-declared-as-external) | Optimization | High



## Right-To-Left-Override character
### Configuration
* Check: `rtlo`
* Severity: `High`
* Confidence: `High`

### Description
An attacker can manipulate the logic of the contract by using a right-to-left-override character (U+202E)

### Exploit Scenario:

```solidity
contract Token
{

    address payable o; // owner
    mapping(address => uint) tokens;

    function withdraw() external returns(uint)
    {
        uint amount = tokens[msg.sender];
        address payable d = msg.sender;
        tokens[msg.sender] = 0;
        _withdraw(/*owner‮/*noitanitsed*/ d, o/*‭
		        /*value */, amount);
    }

    function _withdraw(address payable fee_receiver, address payable destination, uint value) internal
    {
		fee_receiver.transfer(1);
		destination.transfer(value);
    }
}
```

`Token` uses the right-to-left-override character when calling `_withdraw`. As a result, the fee is incorrectly sent to `msg.sender`, and the token balance is sent to the owner.



### Recommendation
Special control characters must not be allowed.

## State variable shadowing
### Configuration
* Check: `shadowing-state`
* Severity: `High`
* Confidence: `High`

### Description
Detection of state variables shadowed.

### Exploit Scenario:

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
Remove the state variable shadowing.

## Suicidal
### Configuration
* Check: `suicidal`
* Severity: `High`
* Confidence: `High`

### Description
Unprotected call to a function executing `selfdestruct`/`suicide`.

### Exploit Scenario:

```solidity
contract Suicidal{
    function kill() public{
        selfdestruct(msg.sender);
    }
}
```
Bob calls `kill` and destructs the contract.

### Recommendation
Protect access to all sensitive functions.

## Uninitialized state variables
### Configuration
* Check: `uninitialized-state`
* Severity: `High`
* Confidence: `High`

### Description
Uninitialized state variables.

### Exploit Scenario:

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
### Configuration
* Check: `uninitialized-storage`
* Severity: `High`
* Confidence: `High`

### Description
An uinitialized storage variable will act as a reference to the first state variable, and can override a critical variable.

### Exploit Scenario:

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
### Configuration
* Check: `arbitrary-send`
* Severity: `High`
* Confidence: `Medium`

### Description
Unprotected call to a function executing sending ethers to an arbitrary address.

### Exploit Scenario:

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

## Controlled Delegatecall
### Configuration
* Check: `controlled-delegatecall`
* Severity: `High`
* Confidence: `Medium`

### Description
Delegatecall or callcode to an address controlled by the user.

### Exploit Scenario:

```solidity
contract Delegatecall{
    function delegate(address to, bytes data){
        to.delegatecall(data);
    }
}
```
Bob calls `delegate` and delegates the execution to its malicious contract. As a result, Bob withdraws the funds of the contract and destructs it.

### Recommendation
Avoid using `delegatecall`. Use only trusted destinations.

## Reentrancy vulnerabilities
### Configuration
* Check: `reentrancy-eth`
* Severity: `High`
* Confidence: `Medium`

### Description

Detection of the [re-entrancy bug](https://github.com/trailofbits/not-so-smart-contracts/tree/master/reentrancy).
Do not report reentrancies that don't involve ethers (see `reentrancy-no-eth`)

### Exploit Scenario:

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

## Incorrect erc20 interface
### Configuration
* Check: `erc20-interface`
* Severity: `Medium`
* Confidence: `High`

### Description
Incorrect return values for ERC20 functions. A contract compiled with solidity > 0.4.22 interacting with these functions will fail to execute them, as the return value is missing.

### Exploit Scenario:

```solidity
contract Token{
    function transfer(address to, uint value) external;
    //...
}
```
`Token.transfer` does not return a boolean. Bob deploys the token. Alice creates a contract that interacts with it but assumes a correct ERC20 interface implementation. Alice's contract is unable to interact with Bob's contract.

### Recommendation
Set the appropriate return values and value-types for the defined ERC20 functions.

## Incorrect erc721 interface
### Configuration
* Check: `erc721-interface`
* Severity: `Medium`
* Confidence: `High`

### Description
Incorrect return values for ERC721 functions. A contract compiled with solidity > 0.4.22 interacting with these functions will fail to execute them, as the return value is missing.

### Exploit Scenario:

```solidity
contract Token{
    function ownerOf(uint256 _tokenId) external view returns (bool);
    //...
}
```
`Token.ownerOf` does not return an address as ERC721 expects. Bob deploys the token. Alice creates a contract that interacts with it but assumes a correct ERC721 interface implementation. Alice's contract is unable to interact with Bob's contract.

### Recommendation
Set the appropriate return values and value-types for the defined ERC721 functions.

## Dangerous strict equalities
### Configuration
* Check: `incorrect-equality`
* Severity: `Medium`
* Confidence: `High`

### Description
Use of strict equalities that can be easily manipulated by an attacker.

### Exploit Scenario:

```solidity
contract Crowdsale{
    function fund_reached() public returns(bool){
        return this.balance == 100 ether;
    }
```
`Crowdsale` relies on `fund_reached` to know when to stop the sale of tokens. `Crowdsale` reaches 100 ether. Bob sends 0.1 ether. As a result, `fund_reached` is always false and the crowdsale never ends.

### Recommendation
Don't use strict equality to determine if an account has enough ethers or tokens.

## Contracts that lock ether
### Configuration
* Check: `locked-ether`
* Severity: `Medium`
* Confidence: `High`

### Description
Contract with a `payable` function, but without a withdraw capacity.

### Exploit Scenario:

```solidity
pragma solidity 0.4.24;
contract Locked{
    function receive() payable public{
    }
}
```
Every ether sent to `Locked` will be lost.

### Recommendation
Remove the payable attribute or add a withdraw function.

## State variable shadowing from abstract contracts
### Configuration
* Check: `shadowing-abstract`
* Severity: `Medium`
* Confidence: `High`

### Description
Detection of state variables shadowed from abstract contracts.

### Exploit Scenario:

```solidity
contract BaseContract{
    address owner;
}

contract DerivedContract is BaseContract{
    address owner;
}
```
`owner` of `BaseContract` is shadowed in `DerivedContract`.

### Recommendation
Remove the state variable shadowing.

## Constant functions changing the state
### Configuration
* Check: `constant-function`
* Severity: `Medium`
* Confidence: `Medium`

### Description

Functions declared as `constant`/`pure`/`view` changing the state or using assembly code.

`constant`/`pure`/`view` was not enforced prior Solidity 0.5.
Starting from Solidity 0.5, a call to a `constant`/`pure`/`view` function uses the `STATICCALL` opcode, which reverts in case of state modification.

As a result, a call to an [incorrectly labeled function may trap a contract compiled with Solidity 0.5](https://solidity.readthedocs.io/en/develop/050-breaking-changes.html#interoperability-with-older-contracts).

### Exploit Scenario:

```solidity
contract Constant{
    uint counter;
    function get() public view returns(uint){
       counter = counter +1;
       return counter
    }
}
```
`Constant` was deployed with Solidity 0.4.25. Bob writes a smart contract interacting with `Constant` in Solidity 0.5.0. 
All the calls to `get` revert, breaking Bob's smart contract execution.

### Recommendation
Ensure that the attributes of contracts compiled prior to Solidity 0.5.0 are correct.

## Reentrancy vulnerabilities
### Configuration
* Check: `reentrancy-no-eth`
* Severity: `Medium`
* Confidence: `Medium`

### Description

Detection of the [re-entrancy bug](https://github.com/trailofbits/not-so-smart-contracts/tree/master/reentrancy).
Do not report reentrancies that involve ethers (see `reentrancy-eth`)

### Exploit Scenario:

```solidity
    function bug(){
        require(not_called);
        if( ! (msg.sender.call() ) ){
            throw;
        }
        not_called = False;
    }   
```


### Recommendation
Apply the [check-effects-interactions pattern](http://solidity.readthedocs.io/en/v0.4.21/security-considerations.html#re-entrancy).

## Dangerous usage of `tx.origin`
### Configuration
* Check: `tx-origin`
* Severity: `Medium`
* Confidence: `Medium`

### Description
`tx.origin`-based protection can be abused by malicious contract if a legitimate user interacts with the malicious contract.

### Exploit Scenario:

```solidity
contract TxOrigin {
    address owner = msg.sender;

    function bug() {
        require(tx.origin == owner);
    }
```
Bob is the owner of `TxOrigin`. Bob calls Eve's contract. Eve's contract calls `TxOrigin` and bypasses the `tx.origin` protection.

### Recommendation
Do not use `tx.origin` for authorization.

## Unchecked low-level calls
### Configuration
* Check: `unchecked-lowlevel`
* Severity: `Medium`
* Confidence: `Medium`

### Description
The return value of a low-level call is not checked.

### Exploit Scenario:

```solidity
contract MyConc{
    function my_func(address payable dst) public payable{
        dst.call.value(msg.value)("");
    }
}
```
The return value of the low-level call is not checked. As a result if the callfailed, the ether will be locked in the contract.
If the low level is used to prevent blocking operations, consider logging failed calls.
    

### Recommendation
Ensure that the return value of low-level call is checked or logged.

## Unchecked Send
### Configuration
* Check: `unchecked-send`
* Severity: `Medium`
* Confidence: `Medium`

### Description
The return value of a send is not checked.

### Exploit Scenario:

```solidity
contract MyConc{
    function my_func(address payable dst) public payable{
        dst.send(msg.value);
    }
}
```
The return value of `send` is not checked. As a result if the send failed, the ether will be locked in the contract.
If `send` is used to prevent blocking operations, consider logging the failed sent.
    

### Recommendation
Ensure that the return value of send is checked or logged.

## Uninitialized local variables
### Configuration
* Check: `uninitialized-local`
* Severity: `Medium`
* Confidence: `Medium`

### Description
Uninitialized local variables.

### Exploit Scenario:

```solidity
contract Uninitialized is Owner{
    function withdraw() payable public onlyOwner{
        address to;
        to.transfer(this.balance)
    }
}
```
Bob calls `transfer`. As a result, the ethers are sent to the address 0x0 and are lost.

### Recommendation
Initialize all the variables. If a variable is meant to be initialized to zero, explicitly set it to zero.

## Unused return
### Configuration
* Check: `unused-return`
* Severity: `Medium`
* Confidence: `Medium`

### Description
The return value of an external call is not stored in a local or state variable.

### Exploit Scenario:

```solidity
contract MyConc{
    using SafeMath for uint;   
    function my_func(uint a, uint b) public{
        a.add(b);
    }
}
```
`MyConc` calls `add` of SafeMath, but does not store the result in `a`. As a result, the computation has no effect.

### Recommendation
Ensure that all the return values of the function calls are used.

## Builtin Symbol Shadowing
### Configuration
* Check: `shadowing-builtin`
* Severity: `Low`
* Confidence: `High`

### Description
Detection of shadowing built-in symbols using local variables/state variables/functions/modifiers/events.

### Exploit Scenario:

```solidity
pragma solidity ^0.4.24;

contract Bug {
    uint now; // Overshadows current time stamp.

    function assert(bool condition) public {
        // Overshadows built-in symbol for providing assertions.
    }

    function get_next_expiration(uint earlier_time) private returns (uint) {
        return now + 259200; // References overshadowed timestamp.
    }
}
```
`now` is defined as a state variable, and shadows with the built-in symbol `now`. The function `assert` overshadows the built-in `assert` function. Any use of either of these built-in symbols may lead to unexpected results.

### Recommendation
Rename the local variable/state variable/function/modifier/event, so as not to mistakenly overshadow any built-in symbol definitions.

## Local Variable Shadowing
### Configuration
* Check: `shadowing-local`
* Severity: `Low`
* Confidence: `High`

### Description
Detection of shadowing using local variables.

### Exploit Scenario:

```solidity
pragma solidity ^0.4.24;

contract Bug {
    uint owner;

    function sensitive_function(address owner) public {
        // ...
        require(owner == msg.sender);
    }

    function alternate_sensitive_function() public {
        address owner = msg.sender;
        // ...
        require(owner == msg.sender);
    }
}
```
`sensitive_function.owner` shadows `Bug.owner`. As a result, the use of `owner` inside `sensitive_function` might be incorrect.

### Recommendation
Rename the local variable so as not to mistakenly overshadow any state variable/function/modifier/event definitions.

## Void Constructor
### Configuration
* Check: `void-cst`
* Severity: `Low`
* Confidence: `High`

### Description
Detect the call to a constructor not implemented

### Exploit Scenario:

```solidity
contract A{}
contract B is A{
    constructor() public A(){}
}
```
By reading B's constructor definition, the reader might assume that `A()` initiate the contract, while no code is executed.



### Recommendation
Remove the constructor call.

## Calls inside a loop
### Configuration
* Check: `calls-loop`
* Severity: `Low`
* Confidence: `Medium`

### Description
Calls inside a loop might lead to denial of service attack.

### Exploit Scenario:

```solidity
contract CallsInLoop{

    address[] destinations;

    constructor(address[] newDestinations) public{
        destinations = newDestinations;
    }

    function bad() external{
        for (uint i=0; i < destinations.length; i++){
            destinations[i].transfer(i);
        }
    }

}
```
If one of the destinations has a fallback function which reverts, `bad` will always revert.

### Recommendation
Favor [pull over push](https://github.com/ethereum/wiki/wiki/Safety#favor-pull-over-push-for-external-calls) strategy for external calls.

## Reentrancy vulnerabilities
### Configuration
* Check: `reentrancy-benign`
* Severity: `Low`
* Confidence: `Medium`

### Description

Detection of the [re-entrancy bug](https://github.com/trailofbits/not-so-smart-contracts/tree/master/reentrancy).
Only report reentrancy that acts as a double call (see `reentrancy-eth`, `reentrancy-no-eth`).

### Exploit Scenario:

```solidity
    function callme(){
        if( ! (msg.sender.call()() ) ){
            throw;
        }
        counter += 1
    }   
```

`callme` contains a reentrancy. The reentrancy is benign because it's exploitation would have the same effect as two consecutive calls.

### Recommendation
Apply the [check-effects-interactions pattern](http://solidity.readthedocs.io/en/v0.4.21/security-considerations.html#re-entrancy).

## Block timestamp
### Configuration
* Check: `timestamp`
* Severity: `Low`
* Confidence: `Medium`

### Description
Dangerous usage of `block.timestamp`. `block.timestamp` can be manipulated by miners.

### Exploit Scenario:
"Bob's contract relies on `block.timestamp` for its randomness. Eve is a miner and manipulates `block.timestamp` to exploit Bob's contract.

### Recommendation
Avoid relying on `block.timestamp`.

## Assembly usage
### Configuration
* Check: `assembly`
* Severity: `Informational`
* Confidence: `High`

### Description
The use of assembly is error-prone and should be avoided.

### Recommendation
Do not use evm assembly.

## Deprecated Standards
### Configuration
* Check: `deprecated-standards`
* Severity: `Informational`
* Confidence: `High`

### Description
Detect the usage of deprecated standards (as defined by SWC-111), excluding only `constant` keyword detection on functions.

### Exploit Scenario:

```solidity
contract ContractWithDeprecatedReferences {
    // Deprecated: Change block.blockhash() -> blockhash()
    bytes32 globalBlockHash = block.blockhash(0);

    // Deprecated: Change constant -> view
    function functionWithDeprecatedThrow() public constant {
        // Deprecated: Change msg.gas -> gasleft()
        if(msg.gas == msg.value) {
            // Deprecated: Change throw -> revert()
            throw;
        }
    }

    // Deprecated: Change constant -> view
    function functionWithDeprecatedReferences() public constant {
        // Deprecated: Change sha3() -> keccak256()
        bytes32 sha3Result = sha3("test deprecated sha3 usage");

        // Deprecated: Change callcode() -> delegatecall()
        address(this).callcode();

        // Deprecated: Change suicide() -> selfdestruct()
        suicide(address(0));
    }
}
```

### Recommendation
Replace all uses of deprecated symbols.

## Unindexed ERC20 Event Parameters
### Configuration
* Check: `erc20-indexed`
* Severity: `Informational`
* Confidence: `High`

### Description
Detects that events defined by the ERC20 specification which are meant to have some parameters as `indexed`, are missing the `indexed` keyword.

### Exploit Scenario:

```solidity
contract ERC20Bad {
    // ...
    event Transfer(address from, address to, uint value);
    event Approval(address owner, address spender, uint value);

    // ...
}
```
In this case, Transfer and Approval events should have the 'indexed' keyword on their two first parameters, as defined by the ERC20 specification. Failure to include these keywords will not include the parameter data in the transaction/block's bloom filter. This may cause external tooling searching for these parameters to overlook them, and fail to index logs from this token contract.

### Recommendation
Add the `indexed` keyword to event parameters which should include it, according to the ERC20 specification.

## Low level calls
### Configuration
* Check: `low-level-calls`
* Severity: `Informational`
* Confidence: `High`

### Description
The use of low-level calls is error-prone. Low-level calls do not check for [code existence](https://solidity.readthedocs.io/en/v0.4.25/control-structures.html#error-handling-assert-require-revert-and-exceptions) or call success.

### Recommendation
Avoid low-level calls. Check the call success. If the call is meant for a contract, check for code existence.

## Conformance to Solidity naming conventions
### Configuration
* Check: `naming-convention`
* Severity: `Informational`
* Confidence: `High`

### Description

Solidity defines a [naming convention](https://solidity.readthedocs.io/en/v0.4.25/style-guide.html#naming-conventions) that should be followed.
#### Rules exceptions
- Allow constant variables name/symbol/decimals to be lowercase (ERC20)
- Allow `_` at the beginning of the mixed_case match for private variables and unused parameters.

### Recommendation
Follow the Solidity [naming convention](https://solidity.readthedocs.io/en/v0.4.25/style-guide.html#naming-conventions).

## Different pragma directives are used
### Configuration
* Check: `pragma`
* Severity: `Informational`
* Confidence: `High`

### Description
Detect if different Solidity versions are used.

### Recommendation
Use one Solidity version.

## Incorrect versions of Solidity
### Configuration
* Check: `solc-version`
* Severity: `Informational`
* Confidence: `High`

### Description

Solc frequently releases new compiler versions. Using an old version prevents access to new Solidity security checks.
We recommend avoiding complex pragma statement.

### Recommendation

Use Solidity 0.4.25 or 0.5.3. Consider using the latest version of Solidity for testing the compilation, and a trusted version for deploying.

## Unused state variables
### Configuration
* Check: `unused-state`
* Severity: `Informational`
* Confidence: `High`

### Description
Unused state variable.

### Recommendation
Remove unused state variables.

## Too many digits
### Configuration
* Check: `too-many-digits`
* Severity: `Informational`
* Confidence: `Medium`

### Description

Literals with many digits are difficult to read and review.


### Exploit Scenario:

```solidity
contract MyContract{
    uint 1_ether = 10000000000000000000; 
}
```

While `1_ether` looks like `1 ether`, it is `10 ether`. As a result, its usage is likely to be incorrect.


### Recommendation

Use:
- [Ether suffix](https://solidity.readthedocs.io/en/latest/units-and-global-variables.html#ether-units)
- [Time suffix](https://solidity.readthedocs.io/en/latest/units-and-global-variables.html#time-units), or
- [The scientific notation](https://solidity.readthedocs.io/en/latest/types.html#rational-and-integer-literals)


## State variables that could be declared constant
### Configuration
* Check: `constable-states`
* Severity: `Optimization`
* Confidence: `High`

### Description
Constant state variable should be declared constant to save gas.

### Recommendation
Add the `constant` attributes to the state variables that never change.

## Public function that could be declared as external
### Configuration
* Check: `external-function`
* Severity: `Optimization`
* Confidence: `High`

### Description
`public` functions that are never called by the contract should be declared `external` to save gas.

### Recommendation
Use the `external` attribute for functions never called from the contract.