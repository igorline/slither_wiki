`slither-check-erc` checks for ERC's conformance.

## Features

- Check that all the functions are present
- Check that all the events are present
- Check that functions return the correct type
- Check that functions that must be view are view
- Check that events' parameters are correctly indexed
- Check that the functions emit the events
- Check that the derived contracts do not break the conformance

## ERC supported
- ERC20
- ERC223
- ERC165
- ERC721
- ERC1820
- ERC777

## Usage:
```
slither-check-erc contract.sol ContractName
```
For example, on

```Solidity
contract ERC20{

    event Transfer(address indexed,address,uint256);

    function transfer(address, uint256) public{

        emit Transfer(msg.sender,msg.sender,0);
    }

}
```

The tool will report:

```
# Check ERC20

## Check functions
[ ] totalSupply() is missing 
[ ] balanceOf(address) is missing 
[✓] transfer(address,uint256) is present
	[ ] transfer(address,uint256) -> () should return bool
	[✓] Transfer(address,address,uint256) is emitted
[ ] transferFrom(address,address,uint256) is missing 
[ ] approve(address,uint256) is missing 
[ ] allowance(address,address) is missing 
[ ] name() is missing (optional)
[ ] symbol() is missing (optional)
[ ] decimals() is missing (optional)

## Check events
[✓] Transfer(address,address,uint256) is present
	[✓] parameter 0 is indexed
	[ ] parameter 1 should be indexed
[ ] Approval(address,address,uint256) is missing
```