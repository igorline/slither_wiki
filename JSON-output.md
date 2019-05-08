At the top level, the JSON output provided by slither will appear in the following format:
```json
{ 
 "success": true,
 "error": null, 
 "results": []
}
```
- `success` (boolean): `true` if `results` were output successfully, `false` if an `error` occurred.
- `error` (string | null): If `success` is `false`, this will be a string with relevant error information. Otherwise, it will be `null`.
- `results` (result array, see below): If `success` is `true`, this will be an array populated with relevant slither findings.

A vulnerability/result found in the `results` array above will be of the following format:

```json
{
     "check": "...",
     "impact": "...",
     "confidence": "...",
     "description": "...",
     "elements": [     
      {
          "type": "item0", 
          "item0_additional_info": "...", 
          "source_mapping" : "..."
      },
      {
          "type": "item1",
          "item1_additional_info": "...",
          "source_mapping" : "..."
      }]
}
```
- `check` (string): The detector identifier (see the [list of detectors](https://github.com/trailofbits/slither#detectors))
- `impact` (string): representation of the impact (`High`/ `Medium`/ `Low`/ `Informational`)
- `confidence` (string): representation of the confidence (`High`/ `Medium`/ `Low`)
- `description` (string): output of the slither
- `elements`: (element array, see below): an array of relevant items for this finding which map to some source code.
  - NOTE: When writing a detector, the first element should be carefully chosen to represent the most significant portion of mapped code for the finding (the area of source on which external tooling should primarily focus for the issue).
- `additional_info`: (OPTIONAL, any): Offers additional detector-specific information, does not always exist.

Each element found in `elements` above is of the form:
```json
{
	"type": "...",
	"name": "...",
	"source_mapping": { ... }
}
```
- `type` (string): Refers to the type of element, this can be either: (`contract`, `function`, `variable`, `node`, `pragma`, `enum`, `struct`, `event`).
- `name` (string): Refers to the name of the element. 
  - For `contract`/`function`/`variable`/`enum`/`struct`/`event` types, this refers to the definition name. 
  - For `node` types, this refers to a string representation of any underlying expression. A blank string is used if there is no underlying expression.
  - For `pragma`, this refers to a string representation of the `version` portion of the pragma (ie: `^0.5.0`).
- `source_mapping` (source mapping, see below): Refers to a source mapping object which defines the source range which represents this element.
- `additional_info`: (OPTIONAL, any): Offers additional detector-specific element information, does not always exist.
  - For `pragma` type elements, a `directive` field will be added here, serializing the full pragma directive.

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
- `filename_short`: it is a shorter version of the path, which hides the platform-specific directories (ex: `node_modules`). 
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

