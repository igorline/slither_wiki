## Standard fields
### `check`
```
"check": "slither_flag"
```
Each element has a `check` field, which is the slither flag to run the detector

### `source_mapping`
```
"source_mapping": {
 "filename": "tests/constant.sol",
 "length": 58,
 "lines": [
   5,
   6,
   7
 ],
 "start": 45
}
```

### `expressions`

```
        "expressions": [
            {
                "source_mapping": {...},
                "expression": "the expression..."
            }, 
            ...
        ],
```
- `expressions` is a list

### `function`
```
        "function": {
           { "name": "function_name",
            "source_mapping": { .. }
            }
        },
```


### `functions`
```
        "functions": {
           [
           { "name": "function_name",
            "source_mapping": { .. }}
            ]
        },
```
- `functions` is a list


### `variables`
```
        "variables": [
            {
                "name": "userBalance",
                "source_mapping": {...}
            }
```
- `variables` is a list



## Detectors

### `pragma`
- `check`
- `expressions`

### `constant-function`
- `check`
- `expressions`
- `function`
- `variables`
- `countains_assembly`: `boolean`
- if `contains_assembly`is true, `variables` is empty.

Ex: 
```
[
    {
        "check": "constant-function",
        "contains_assembly": false,
        "functions": [ {
            "name": "test_view_bug",
            "source_mapping": { .. }
        }],
        "variables": [
            "name": "test_view_bug",
            "source_mapping": { .. }
        ]
    }
]
```

### `locked-ether`
- `check`
- `functions`

### `solc-version`
- `check`
- `expressions`


### `arbitrary-send`
- `check`
- `expressions`
- `function`

### `external-function`
- `check`
- `function`


### `suicidal`
- `check`
- `function`

### `naming-convention`
```
[
    {
        "check": "naming-convention",
        "convention": "CapWords",
        "name": {
            "name": "contract_name",
            "source_mapping": {...},
        "type": "contract"
    }
]
```
- `convention` can be:
  - `CapWords`
  - `mixedCase`
  - `l_O_I_should_not_be_used`
  - `UPPER_CASE_WITH_UNDERSCORES`
- `type` can be:
  - `contract`
  - `structure`
  - `event`
  - `function`
  - `variable`
  - `variable_constant`
  - `parameter`
  - `enum`
  - `modifier`

### `low-level-calls`
- `check`
- `expressions`
- `function`

### `unused-return`
- `check`
- `expressions`
- `function`

### `reentrancy`
```
[
    {
        "check": "reentrancy",
        "external_calls": [
            {
                "expression": "! (msg.sender.call.value(userBalance[msg.sender])())",
                "source_mapping": {...}
            }
        ],
        "external_calls_sending_eth": [
            {
                "expression": "! (msg.sender.call.value(userBalance[msg.sender])())",
                "source_mapping": {...}
            }
        ],
        "function": {
            "name": "withdrawBalance",
            "source_mapping": {...}
        },
        "variables": [
            {
                "expression": "userBalance[msg.sender] = 0",
                "name": "userBalance",
                "source_mapping": {...}
            }
        ]
    }
]
```
- `external_calls` contains a list
- `external_calls_sending_eth` contains a list
- `variables` contains a list
- `external_calls_sending_eth` can be empty

### `assembly`

```
[
    {
        "assembly": [
            {
                "source_mapping": {...}
            }
        ],
        "check": "assembly",
        "function": {
            "name": "at",
            "source_mapping": {...}
        }
    }
]
```
- `assembly` contains a list

###  `controlled-delegatecall`
- `check`
- `expressions`
- `function`

### `tx-origin`
- `check`
- `expressions`
- `function`

### `constable-states`
- `check`
- `variables`