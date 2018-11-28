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