Let's start with the smallest possible Slither script:

```python
from slither.slither import Slither # Requires Slither in PYTHONPATH  
  
slither = Slither('file.sol')  
```

A Slither object has:
- `contracts (list(Contract)`: list of contracts
- `contracts_derived (list(Contract)`: list of contracts that are not inherited by another contract (subset of contracts)
- `get_contract_from_name (str)`: Return a contract from its name

`contracts_derived` iterates over contracts that are not inherited. It is useful to prevent duplicate findings. If you find an issue in a derived contract, then one of its inherited contracts is likely to have the same issue.

A Contract object has:
- `name (str)`: Name of the contract
- `functions (list(Function))`: List of functions
- `modifiers (list(Modifier))`: List of functions
- `inheritance (list(Contract))`: List of inherited contracts
- `get_function_from_signature (str)`: Return a `Function` from its signature
- `get_modifier_from_signature (str)`: Return a `Modifier` from its signature
- `get_state_variable_from_name (str)`: Return a `StateVariable` from its name

A Function or a Modifier object has:
- `name (str)`: Name of the function
- `nodes (list(Node))`: List of the nodes composing the CFG of the function/modifier
- `entry_point (Node)`: Entry point of the CFG
- `variables_read (list(Variable))`: List of variables read
- `variables_written (list(Variable))`: List of variables written
- `state_variables_read (list(StateVariable))`: List of state variables read (subset of variables`read)
- `state_variables_written (list(StateVariable))`: List of state variables written (subset of variables`written)

Variables can be different types, such as StateVariable, or LocalVariable. All variables have:
- `name (str)`: Name of the variable
- `initialized (boolean)`: True if the variable is initialized at declaration

A Node object has:
- `type (NodeType)`: The type of the node (ex: If a control flow node, RETURN is for the node containing the return statement).
- `expression (Expression)`: Expression associated with the node (not all nodes contain an expression)
- `variables_read (list(Variable))`: List of variables read
- `variables_written (list(Variable))`: List of variables written
- `state_variables_read (list(StateVariable))`: List of state variables read (subset of variables_read)
- `state_variables_written (list(StateVariable))`: List of state variables written (subset of variables_written)

An Expression is an AST-based representation of the code executed.

For example, the following code explores all the functions of all the contracts and prints what state variables are read or written:

```python
from slither.slither import Slither  
  
slither = Slither('file.sol')  
  
for contract in slither.contracts:  
    print 'Contract: '+ contract.name  
  
    for function in contract.functions:  
        print('Function: {}'.format(function.name))  
  
        print('\tRead: {}'.format([v.name for v in function.state_variables_read]))  
        print('\tWritten {}'.format([v.name for v in function.state_variables_written]))
```

You will find more Slither API examples [here](https://github.com/trailofbits/slither/blob/master/examples/scripts/). For example:
* [functions_writing.py](https://github.com/trailofbits/slither/blob/master/examples/scripts/functions_writing.py): Where the state variable `a` is written?
* [variable_in_condition.py](https://github.com/trailofbits/slither/blob/master/examples/scripts/variable_in_condition.py): Is the variable `a` used in a condition?
* [functions_called.py](https://github.com/trailofbits/slither/blob/master/examples/scripts/functions_called.py): What are all the functions reached by a call to `entry_point()`?
* [slithIR.py](https://github.com/trailofbits/slither/blob/master/examples/scripts/slithIR.py): Print the SlithIR operations 
