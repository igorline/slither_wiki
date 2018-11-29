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


### `variable`
```
        "variable": 
            {
                "name": "userBalance",
                "source_mapping": {...}
            }
```

### `variables`
```
        "variables": [
            {
                "name": "userBalance",
                "source_mapping": {...}
            }
        ]
```
- `variables` is a list



## Detectors

Num | Detector                  | `check` | `function` | `functions` | `variable` | `variables` | `expressions` | Extra 
--- | ---                       | :---:   | :---:      | :---:       | :---:       | :---:       | :---:         |  --- 
1 | `suicidal`                  |    X    |     X      |             |             |             |               |  
2 | `uninitialized-state`       |    X    |            |     X       |    X        |             |               | 
3 | `uninitialized-storage`     |    X    |     X      |             |    X        |             |               | 
4 | `arbitrary-send`            |    X    |     X      |             |             |             |      X        | 
5 | `controlled-delegatecall`   |    X    |     X      |             |             |             |      X        | 
6 | `reentrancy`                |    X    |            |             |             |             |               | Yes 
7 | `locked-ether`              |    X    |     X      |             |             |             |               | 
8 | `constant-function`         |    X    |     X      |             |    X        |             |     X         | Yes
9 | `tx-origin`                 |    X    |     X      |             |             |             |     X         | 
10 | `uninitialized-local`      |    X    |     X      |             |    X        |             |               | 
11 | `unused-return`            |    X    |     X      |             |             |             |     X         | 
12 | `assembly`                 |    X    |     X      |             |             |             |               | Yes
13 | `constable-states`         |    X    |     X      |             |             |     X       |               | 
14 | `external-function`        |    X    |     X      |             |             |             |               | 
15 | `low-level-calls`          |    X    |     X      |             |             |             |     X         | 
16 | `naming-convention`        |    X    |            |             |             |             |               | Yes
17 | `pragma`                   |    X    |            |             |             |             |     X         | 
18 | `solc-version`             |    X    |            |             |             |             |     X         | 
19 | `unused-state`             |    X    |            |             |             |     X       |               |                 

## Exceptions

### `constant-function`
The additional field is the boolean `contain_assembly`

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
- if `contains_assembly`is true, `variables` is empty.


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
        "variables_written": [
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

