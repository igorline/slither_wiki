## Slither API

```python
from slither import Slither  
  
slither = Slither('example.sol')  
```

For a `Slither` object, you can access:

- `slither.pragma_directives`: list of the [Pragma](https://github.com/trailofbits/slither/blob/master/slither/core/declarations/pragma_directive.py) directives
- `slither.import_directives`: list of the [Import](https://github.com/trailofbits/slither/blob/master/slither/core/declarations/import_directive.py) directives
- `slither.source_code`: a dictionary that maps a filename to its source code
- `slither.solc_version`: the solc version that was detected 

The solc version is detected either by running `solc --version` or by looking at the version provided by Truffle.

The contracts are acceded through: 
- `slither.contracts`: list of all the contracts
- `slither.contracts_derived`:  list of all the contracts that are not inherited
- `slither.get_contract_from_name(name)`: get a contract from a given name (return None if it is not found)


## Contract information



