`slither-flat` produces a flattened version of the codebase.

`slither-flat` is experimental. Please [report](https://github.com/crytic/slither/issues) any issues encountered.

## Usage
`slither-flat target`

- `--convert-external`: convert `external` function to `public`. This is meant to facilitate [Echidna](https://github.com/crytic/echidna) usage.
- `--contract name`:  To flatten only a target contract
- `--remove-assert`: Remove call to assert().

## Features
- Export one file per most derived contracts
- Support circular dependency
- Support all the compilation platforms (Truffle, embark, buidler, etherlime, ...).