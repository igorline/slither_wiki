# Upgradeability Checks

`slither-check-upgradability` is meant to help to review contracts using the [delegatecall proxy pattern](https://blog.trailofbits.com/2018/09/05/contract-upgrade-anti-patterns/).

The tool checks that:
- There is no function id collision
- The order of variables is the same
- The initialization is correct

## Usage
```
slither-check-upgradability proxy.sol ProxyName implem.sol ContractName
```

If a second version of the contract is to be checked, use:

```
slither-check-upgradeability proxy.sol ProxyName implem.sol ContractName implem_v2.sol ContractNameV2
```

`proxy.sol`/`implem.sol` can be a Truffle directory, for example: 
```
slither-check-upgradability . ProxyName . ContractName
```

### Proxy contract
If you use zos, you will have the proxy and the contract in different directory and Solidity version.
To allow `slither-check-upgradability` to work, you can compile manually the proxy contract to its AST form (`solc file.sol --ast-compact-json > file.json`), and provides the generated file to `slither-check-upgradability`. Use [`solc-select`](https://github.com/crytic/solc-select) to switch between Solidity version.

For example:
```
cd my_contracts
cd node_modules/zos-lib
npm install openzeppelin-solidity
cp node_modules/openzeppelin-solidity/ . -r
solc contracts/upgradeability/AdminUpgradeabilityProxy.sol  --allow-paths . --ast-compact-json > AdminUpgradeabilityProxy.json
cd ..
slither-check-upgradability node_modules/openzeppelin-solidity/contracts/upgradeability/AdminUpgradeabilityProxy/json AdminUpgradeabilityProxy . MyImplementation
```

## Checks

### Functions ids checks
`slither-check-upgradeability` checks that:
- There is no function id collision between the proxy and the implementation
- The proxy does not shadow any functions from the implementation

### Variables order checks
`slither-check-upgradeability` checks that:
- The variables are declared in the same order between the proxy and the implementation
- The variables are declared in the implementation first and second version

`slither-check-upgradeability` will warn if the proxy has state variables. Consider using the Unstructured storage pattern for those variables. 

### Initialization checks

Contracts based on `delegatecallproxy` cannot be initialized through constructors. As a result, init functions must be used. These functions do not benefitfrom:
- The Solidity C3 linearization. As a result, an init function can be called multiple times, or never.
- The init functions must be calleable only once.
- Race conditions can occur during deployement.

If the contract uses the zos library (and the `Initializable` contract), the util checks that:
 - All the `initalize` functions call the `initializer` modifier 
 - All the inherits `initalize` functions are called
 - `initialize` functions are never called twice

Additionally, `slither-check-upgradeability` prints the functions that must be called during initialization.

If the contract uses another schema, no check is performed. In that case, the tool warns:
```
Initializable contract not found, the contract does not follow a standard initalization schema.
```


