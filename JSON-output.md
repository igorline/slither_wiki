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

`constant-function`
```
[
    {
        "check": "constant-function",
        "contains_assembly": false,
        "function": {
            "name": "test_view_bug",
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
        },
        "variables_written": [
            "a"
        ]
    }
]
```
> if `contains_assembly`is true, `variables_written` is empty.