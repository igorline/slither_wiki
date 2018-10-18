# SlithIR
Slither possesses an intermediate representation, called SlithIR.

SlithIR allows:
- To simplify code analysis
- To write more precise detectors
- To support taint and value tracking 

SlihtIR is an ongoing project, [contact us](https://www.trailofbits.com/contact/) for more information.


## Variables
- `StateVariable`
- `LocalVariable`
- `Constant` (`string` or `int`)
- `SolidityVariable`
- `TupleVariable`
- `TemporaryVariable` (variables added by slithIR)
- `ReferenceVariable` (variables added by slithIR, for mapping/index accesses)

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

