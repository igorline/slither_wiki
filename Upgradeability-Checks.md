# Upgradeability Checks

`slither-check-upgradability` is meant to help to review contracts using the [delegatecall proxy pattern](https://blog.trailofbits.com/2018/09/05/contract-upgrade-anti-patterns/).

The tool checks that:
- There is no function id collision
- The order of variables is the same
- The initialization is correct (if `Initializable` is used):
   - All the `initalize` functions call the `initializer` modifier 
   - All the inherits `initalize` functions are called
   - `initalize` functions are never called twice

## Usage
```
slither-check-upgradability proxy.sol ProxyName implem.sol ContractName
```

`proxy.sol`/`implem.sol` can be a Truffle directory. If your proxy and your implementation are in different directories and the solc compilation is complex, you can first compile the contract to its ast compact json output (`solc file.sol --ast-compact-json > file.json`), and use the generated json file (instead of `file.sol`).

If a second version of the contract is to be checked, use:

```
slither-check-upgradability proxy.sol ProxyName implem.sol ContractName implem_v2.sol ContractNameV2
```