## Top-level Output Format
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

## Vulnerability Results/Findings
A vulnerability result found in the `results` array above will be of the following format:

```
{
     "check": "...",
     "impact": "...",
     "confidence": "...",
     "description": "...",
     "elements": []
}
```
- `check` (string): The detector identifier (see the [list of detectors](https://github.com/trailofbits/slither#detectors))
- `impact` (string): representation of the impact (`High`/ `Medium`/ `Low`/ `Informational`)
- `confidence` (string): representation of the confidence (`High`/ `Medium`/ `Low`)
- `description` (string): output of the slither
- `elements`: (element array, see below): an array of relevant items for this finding which map to some source code.
  - NOTE: When writing a detector, the first element should be carefully chosen to represent the most significant portion of mapped code for the finding (the area of source on which external tooling should primarily focus for the issue).
- `additional_fields`: (OPTIONAL, any): Offers additional detector-specific information, does not always exist.

## Vulnerability Result Elements
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
  - For `pragma` types, this refers to a string representation of the `version` portion of the pragma (ie: `^0.5.0`).
- `source_mapping` (source mapping, see below): Refers to a source mapping object which defines the source range which represents this element.
- `parent`: (OPTIONAL, result-element):
  - For `function`/`enum`/`struct`/`event` type elements: The parent contract of the function.
  - For `variable` type elements: 
    - If state variable: The parent contract
    - If local variable: The parent function
  - For `node` type elements: The parent function of the node.
  - For `pragma` type elements: Fully serialized pragma directive (ie: `["solidity", "^", "0.4", ".9"]`)
- `additional_fields`: (OPTIONAL, any): Offers additional detector-specific element information, does not always exist.

## Source Mapping
Each `source_mapping` object is used to map an element to some portion of source. It is of the form:
```
"source_mapping": {
 "start": 45
 "length": 58,
 "filename_relative": "contracts/tests/constant.sol",
 "filename_absolute": "/tmp/contracts/tests/constant.sol",
 "filename_short": "tests/constant.sol",
 "filename_used": "contracts/tests/constant.sol",
 "lines": [
   5,
   6,
   7
 ],
 "starting_column": 1,
 "ending_column": 24,
}
```
- `start` (integer): Refers to the starting byte position of the mapped source.
- `length` (integer): Refers to the byte-length of the mapped source.
- `filename_relative` (string): A relative file path from the analysis directory.
- `filename_absolute` (string): An absolute file path to the file.
- `filename_short` (string): A short version of the filename used for display purposes. Hides platform-specific directories (ex: `node_modules`).
- `filename_used` (string): The path used by the platform for analysis (non-standard).
- `lines` (integer array): An array of line numbers which the mapped source spans. Line numbers begin from 1.
- `starting_column` (integer): The starting column/character position for the first mapped source line. Begins from 1.
- `ending_column` (integer): The ending column/character position for the last mapped source line. Begins from 1.

## Detector-specific additional fields
Some detectors have custom elements output via the `additional_fields` field of their result, or result elements. Annotations here will specify _result_ or _result-element_ to specify the location of the additional fields.
- `constant-function`: 
  - `contain_assembly` (result, boolean): Specifies if the result is due to the function containing assembly.
- `naming-convention`: 
  - `convention` (result-element, string): Used to denote the convention used to find the result element/issue. Valid conventions are:
    - `CapWords`
    - `mixedCase`
    - `l_O_I_should_not_be_used`
    - `UPPER_CASE_WITH_UNDERSCORES`
  - `target` (result-element, string): Used to denote the type of finding (constant, parameter, etc). Valid targets are:
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
  - `underlying_type` (result-element, string): Specifies the type of result element. Is one of `external_calls`, `external_calls_sending_eth`, or `variables_written`.