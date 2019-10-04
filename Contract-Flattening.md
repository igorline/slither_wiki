`slither-flat` produces a flattened version of the codebase.

## Usage
`slither-flat target`

- `--convert-external`: convert `external` function to `public`. This is meant to facilitate [Echidna](https://github.com/crytic/echidna) usage.
- `--contract name`:  To flatten only a target contract