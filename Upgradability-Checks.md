# Upgradability Checks

`slither-check-upgradability` is meant to help to review contracts using the [delegatecall proxy pattern](https://blog.trailofbits.com/2018/09/05/contract-upgrade-anti-patterns/).

The tool checks that:
- there is no function id collision
- the order of variables is the same

## Usage
```
slither-check-upgradability proxy.sol ProxyName implem.sol ContractName
```

If a second version of the contract is to be checked, use:

```
slither-check-upgradability proxy.sol ProxyName implem.sol ContractName implem_v2.sol ContractNameV2
```

