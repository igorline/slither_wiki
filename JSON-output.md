## Json Output
- Each element has a `check` field, which is the slither flag to run the detector
- If possible, there is a `source_mapping` field to match the information to the source code

## Source mapping format
In the following, each `source_mapping` field follows this format:
```
{
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

## Detectors

### `pragma`
```
[
    {
        "pragmas": [
            {
                "source_mapping": {...},
                "version": "^0.4.23"
            }, 
            ...
        ],
        "vuln": "pragma"
    }
]
```
- `pragmas` contains a list

### `constant-function`
```
[
    {
        "check": "constant-function",
        "contains_assembly": false,
        "function": {
            "name": "test_view_bug",
            "source_mapping": { .. }
        },
        "variables_written": [
            "a"
        ]
    }
]
```
- `variables_written` contains a list
- if `contains_assembly`is true, `variables_written` is empty.

### `locked-ether`
```
[
    {
        "check": "locked-ether",
        "functions_payable": [
            {
                "name": "receive",
                "source_mapping": {...}
            }
        ]
    }
]
```
- `functions_payable` contains a list

### `solc-version`
```
[
    {
        "check": "solc-version",
        "pragmas": [
            {
                "source_mapping": {...},
                "version": "0.4.21"
            }
        ]
    }
]
```
- `pragmas` contains a list

### `arbitrary-send`
```
[
    {
        "check": "arbitrary-send",
        "dangerous_calls": [
            {
                "source_mapping": {...}
            }
        ],
        "function": {
            "name": "direct",
            "source_mapping": {...}
        }
    }
]
```
- `dangerous_calls` contains a list

### `external-function`
```
[
    {
        "check": "external-function",
        "function": {
            "name": "myfunc",
            "source_mapping": {..}
        }
    }
}
```

### `suicidal`
```
[
    {
        "check": "suicidal",
        "function": {
            "name": "i_am_a_backdoor",
            "source_mapping": {...}
        }
    }
]
```

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
```
[
    {
        "check": "low-level-calls",
        "function": {
            "name": "send",
            "source_mapping": {...}
        },
        "low_level_calls": [
            {
                "expression": "_receiver.call.value(msg.value).gas(7777)()",
                "source_mapping": {...}
            }
        ]
    }
]
```
- `low_level_calls` contains a list

### `unused-return`

```
[
    {
        "check": "unused-return",
        "function": {
            "name": "test",
            "source_mapping": {...}
        },
        "unused_returns": [
            {
                "expression": "a.add(0)",
                "source_mapping": {...}
            }
        ]
    }
]
```
- `unused_returns` contains a list

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
- `variables_written` contains a list
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

```
[
    {
        "check": "controlled-delegatecall",
        "controlled_delegatecalls": [
            {
                "expression": "addr_bad.delegatecall(func_id,data)",
                "source_mapping": {...}
            }
        ],
        "function": {
            "name": "bad_delegate_call2",
            "source_mapping": {...}
        }
    }
]
```
- `controlled_delegatecalls` contains a list
