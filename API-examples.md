The minimal Slither script is:

```python
from slither.slither import Slither # note: require slither directory to be in PYTHONPATH  
  
slither = Slither('file.sol')  
```

A slither object has:
- _contracts (list(Contract)_: list of contracts
- _contracts_derived (list(Contract)_: list of contracts that are not inherited by another contract (subset of contracts)
- _get_contract_from_name (str)_: Return a contract from its name

_contracts_derived_ allows to iterate only over the contracts that are not inherited. Its useful to prevent duplicates findings:  if you find an issue in a devired contract, one of its inherited contracts is likely to have the same issue.

A Contract object has:
- _name (str)_: Name of the contract
- _functions (list(Function))_: List of functions
- _modifiers (list(Modifier))_: List of functions
- _inheritances (list(Contract))_: List of contracts inherited
- _get_function_from_signature (str)_: Return a _Function_ from its signature
- _get_modifier_from_signature (str)_: Return a _Modifier_ from its signature
- _get_state_variable_from_name (str)_: Return a _StateVariable_ from its name

A Function or a Modifier object has:
- _name (str)_: Name of the function
- _nodes (list(Node))_: List of the nodes composing the CFG of the function/modifier
- _entry_point (Node)_: Entry point of the CFG
- _variables_read (list(Variable))_: List of variables read
- _variables_written (list(Variable))_: List of variables written
- _state_variables_read (list(StateVariable))_: List of state variables read (subset of variables_read)
- _state_variables_written (list(StateVariable))_: List of state variables written (subset of variables_written)

Variables can be of different types, such as StateVariable, or LocalVariable. All variables have:
- _name (str)_: Name of the variable
- _initialized (boolean)_: True if the variable is initialized at declaration

A Node object has:
- _type (NodeType)_: Define the type of the node (ex: IF is a control flow node, RETURN is for node containing the return statement).
- _expression (Expression)_: Expression associated to the node (all nodes do not contain an expression)
- _variables_read (list(Variable))_: List of variables read
- _variables_written (list(Variable))_: List of variables written
- _state_variables_read (list(StateVariable))_: List of state variables read (subset of variables_read)
- _state_variables_written (list(StateVariable))_: List of state variables written (subset of variables_written)

An Expression is an AST-based representation of the code executed.

For example, the following code explores all the function of all the contracts and print what state variables are read or written:

```python
from slither.slither import Slither  
  
slither = Slither('file.sol')  
  
for contract in slither.contracts:  
    print 'Contract: '+ contract.name  
  
    for function in contract.functions:  
        print 'Function: {}'.format(function.name)  
  
        print '\tRead: {}'.format([v.name for v in function.state_variables_read])  
        print '\tWritten {}'.format([v.name for v in function.state_variables_written])
```

You will find more examples on Slither API [here](https://github.com/trailofbits/slither/tree/f47c4385db33c09d26e3dc67b20d58ec80995f91/examples/scripts). For example:
- [functions_writing.py](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/examples/scripts/functions_writing.py): Where the state variable `a` is written?
- [variable_in_condition.py](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/examples/scripts/variable_in_condition.py): Is the variable `a` used in a condition?
- [functions_called.py](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/examples/scripts/functions_called.py): What are all the functions reached by a call to `entry_point()`?
