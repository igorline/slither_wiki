The json contains a list of vulnerability. A vulnerability is described following this format:

```
{
     check: ...
     impact: ...
     confidence: ...
     description: ...
     elements: [     
      {
          type: item0 
          item0_additional_info: ...  
          source_mapping : ...
      },
      {
          type: item1
          item1_additional_info: ...  
          source_mapping : ...
      }
}
```
- `check`: slither flag (see the [list of flags](https://github.com/trailofbits/slither#detectors))
- `impact`: string representation of the impact (`High`/ `Medium`/ `Low`/ `Informational`)
- `confidence`: string representation of the confidence (`High`/ `Medium`/ `Low`)
- `description`: string output of slither
- `elements`: structure that changes according to the vulnerability class. Each element has at least its `type` (described below) and a `source_mapping` information. As a result, the additional info can be skiped to facilitate the parsing of the json

`source_mapping` is:
```
"source_mapping": {
 "filename_relative": "contracts/tests/constant.sol",
 "filename_absolute": "/tmp/contracts/tests/constant.sol",
 "filename_short": "tests/constant.sol",
 "filename_used": "contracts/tests/constant.sol",
 "length": 58,
 "lines": [
   5,
   6,
   7
 ],
 "start": 45
}
```

Notes:
- `filename_short`: it is a shorted version of the path, which hides the platform-specific directories (ex: `node_modules`). 
- `filename_used`: the path used by the platform. Its format is non-standard


## Expressions types
- type `contract` has
  - `"name":"contract_name"` 
- `function` has
  - `"name": "function_name"`
  - `"contract": type contract`
- `functions` has
  - list of function
- `variable` has 
   - `"name": "variable_name"`
- `variables` has
  - list of variable
- `expression` has
  - `expression`: a string representation of the expression

## Additional types
Some detectors have non standard elements
- `constant-function`: `contain_assembly`: bool
- `naming-convention`: "convention": "CapWords", "name": "contract_name", "target": "target_name"
  - `convention` can be:
    - `CapWords`
    - `mixedCase`
    - `l_O_I_should_not_be_used`
    - `UPPER_CASE_WITH_UNDERSCORES`
  - `target` can be:
    - `contract`
    - `structure`
    - `event`
    - `function`
    - `variable`
    - `variable_constant`
    - `parameter`
    - `enum`
    - `modifier`

- `reentrancy` (all variants): 
  - list of "external_calls": `expression`/`source_mapping`
  - list of "external_calls_sending_eth": `expression`/`source_mapping` 
  - list of "variables_written": `expression`/`source_mapping`/`name`

