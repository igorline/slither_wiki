# SlithIR
Slither translates Solidity an intermediate representation, SlithIR, to enable high-precision analysis via a simple API. It supports taint and value tracking to enable detection of complex patterns.

SlithIR is a work in progress, although it is usable today. New developments in SlithIR are driven by needs identified by new detector modules. Please help us bugtest and enhance SlithIR!

## Example
`$ slither file.sol --printer-slithir` will output the IR for every function.

```
$ slither examples/printers/slihtir.sol --printer-slithir
Contract UnsafeMath
	Function add(uint256,uint256)
		Expression: a + b
		IRs:
			TMP_0(uint256) = a + b
			RETURN TMP_0
	Function min(uint256,uint256)
		Expression: a - b
		IRs:
			TMP_0(uint256) = a - b
			RETURN TMP_0
Contract MyContract
	Function transfer(address,uint256)
		Expression: balances[msg.sender] = balances[msg.sender].min(val)
		IRs:
			REF_3(uint256) -> balances[msg.sender]
			REF_1(uint256) -> balances[msg.sender]
			TMP_1(uint256) = LIBRARY_CALL, dest:UnsafeMath, function:min, arguments:['REF_1', 'val'] 
			REF_3 := TMP_1
		Expression: balances[to] = balances[to].add(val)
		IRs:
			REF_3(uint256) -> balances[to]
			REF_1(uint256) -> balances[to]
			TMP_1(uint256) = LIBRARY_CALL, dest:UnsafeMath, function:add, arguments:['REF_1', 'val'] 
			REF_3 := TMP_1
```

# SlithIR Specification 

## Variables
- `StateVariable`
- `LocalVariable`
- `Constant` (`string` or `int`)
- `SolidityVariable`
- `TupleVariable`
- `TemporaryVariable` (variables added by SlithIR)
- `ReferenceVariable` (variables added by SlithIR, for mapping/index accesses)

In the following we use:
- a `LVALUE` can be: `StateVariable`, `LocalVariable`, `TemporaryVariable`, `ReferenceVariable` or `TupleVariable`

- a `RVALUE` can be: `StateVariable`, `LocalVariable`, `Constant`, `SolidityVariable`, `TemporaryVariable` or `ReferenceVariable` 

## Operators

- All the operators inherit from `Operation` and have a `read` attribute returning the list of variables read (see [slither/slithir/operations/operation.py](https://github.com/trailofbits/slither/blob/slithir/slither/slithir/operations/operation.py)).

- All the operators writing to a `LVALUE` inherit from `OperationWithLValue` and have the `lvalue` attribute (see [slither/slithir/operations/lvalue.py](https://github.com/trailofbits/slither/blob/slithir/slither/slithir/operations/lvalue.py)).

### Assignment

- `LVALUE := RVALUE`
- `LVALUE := Tuple`
- `LVALUE := Function` (for dynamic function)

### Binary Operation

- `LVALUE = RVALUE ** RVALUE`
- `LVALUE = RVALUE * RVALUE`
- `LVALUE = RVALUE / RVALUE`
- `LVALUE = RVALUE % RVALUE`
- `LVALUE = RVALUE + RVALUE`
- `LVALUE = RVALUE - RVALUE`
- `LVALUE = RVALUE << RVALUE`
- `LVALUE = RVALUE >> RVALUE`
- `LVALUE = RVALUE & RVALUE`
- `LVALUE = RVALUE ^ RVALUE`
- `LVALUE = RVALUE | RVALUE`
- `LVALUE = RVALUE < RVALUE`
- `LVALUE = RVALUE > RVALUE`
- `LVALUE = RVALUE <= RVALUE`
- `LVALUE = RVALUE >= RVALUE`
- `LVALUE = RVALUE == RVALUE`
- `LVALUE = RVALUE != RVALUE`
- `LVALUE = RVALUE && RVALUE`
- `LVALUE = RVALUE -- RVALUE`

### Unary Operation
- `LVALUE = ! RVALUE`
- `LVALUE = ~ RVALUE`

### Index
- `REFERENCE -> LVALUE [ RVALUE ]`

Note: The reference points to the memory location

### Member
- `REFERENCE -> LVALUE . RVALUE`
- `REFERENCE -> CONTRACT . RVALUE`
- `REFERENCE -> ENUM . RVALUE`

Note: The reference points to the memory location

### New Operators
- `LVALUE = NEW_ARRAY ARRAY_TYPE DEPTH(:int)`

`ARRAY_TYPE` is a [solidity_types](https://github.com/trailofbits/slither/tree/master/slither/core/solidity_types)

`DEPTH` is used for arrays of multi-dimension)

- `LVALUE = NEW_CONTRACT CONSTANT`
- `LVALUE = NEW_STRUCTURE STRUCTURE`
- `LVALUE = NEW_ELEMENTARY_TYPE ELEMENTARY_TYPE`

`ELEMENTARY_TYPE` is defined in [slither/core/solidity_types/elementary_type.py](https://github.com/trailofbits/slither/blob/master/slither/core/solidity_types/elementary_type.py)

### Push Operator
- `PUSH LVALUE RVALUE`

### Delete Operator
- `DELETE LVALUE`

### Conversion
- `CONVERT LVALUE RVALUE TYPE`

TYPE is a [solidity_types](https://github.com/trailofbits/slither/tree/master/slither/core/solidity_types)

### Unpack
- `LVALUE = UNPACK TUPLEVARIABLE INDEX(:int)`

### Array Initialization
- `LVALUE = INIT_VALUES`

`INIT_VALUES` is a list of `RVALUE`, or a list of list in case of a multi dimensions array.

### Calls Operators

In the following, `ARG` is a variable as defined in [SlithIR#variables](https://github.com/trailofbits/slither-private/wiki/SlithIR#variables)

- `LVALUE = HIGH_LEVEL_CALL DESTINATION FUNCTION [ARG, ..]`
- `LVALUE = LOW_LEVEL_CALL DESTINATION FUNCTION_NAME [ARG, ..]`

`FUNCTION_NAME` can only be `call`/`delegatecall`/`codecall`

- `LVALUE = SOLIDITY_CALL SOLIDITY_FUNCTION [ARG, ..]` 

`SOLIDITY_FUNCTION` is defined in [slither/core/declarations/solidity_variables.py](https://github.com/trailofbits/slither/blob/master/slither/core/declarations/solidity_variables.py)

- `LVALUE = INTERNAL_CALL FUNCTION [ARG, ..]` 
- `LVALUE = INTERNAL_DYNAMIC_CALL FUNCTION_TYPE [ARG, ..]` 

`INTERNAL_DYNAMIC_CALL` represent pointer of function.

`FUNCTION_TYPE` is defined in [slither/core/solidity_types/function_type.py](https://github.com/trailofbits/slither/blob/master/slither/core/solidity_types/function_type.py)

- `LVALUE = LIBRARY_CALL DESTINATION FUNCTION_NAME [ARG, ..]`
- `LVALUE = EVENT_CALL EVENT_NAME [ARG, ..]`
- `LVALUE = SEND DESTINATION VALUE`
- `TRANSFER DESTINATION VALUE`


Optional arguments: 
- `GAS` and `VALUE` for `HIGH_LEVEL_CALL` / `LOW_LEVEL_CALL`. 

### Return
- `RETURN RVALUE`
- `RETURN TUPLE`
- `RETURN None`

`Return None` represents an empty return statement

### Condition
- `CONDITION RVALUE`

`CONDITION` holds the condition in a conditional node.

