## Source mapping example
In the following, each `source_mapping` field follow this format:
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

`pragma`
```json
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

`constant-function`
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