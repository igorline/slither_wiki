# Upgradeability Checks

`slither-check-upgradability` is meant to help to review contracts using the [delegatecall proxy pattern](https://blog.trailofbits.com/2018/09/05/contract-upgrade-anti-patterns/).

The tool checks that:
* [There is no function id collision](https://github.com/crytic/slither/wiki/Upgradeability-Checks#function-id-checks)
* [The order of variables is the same](https://github.com/crytic/slither/wiki/Upgradeability-Checks#variable-order-checks)
* [The initialization is correct](https://github.com/crytic/slither/wiki/Upgradeability-Checks#initialization-checks)

## Usage
```
slither-check-upgradeability proxy.sol ProxyName implem.sol ContractName
```

If a second version of the contract is to be checked, use:

```
slither-check-upgradeability proxy.sol ProxyName implem.sol ContractName implem_v2.sol ContractNameV2
```

`proxy.sol`/`implem.sol` can be a Truffle directory, for example: 
```
slither-check-upgradeability . ProxyName . ContractName
```

### Proxy contract
If you use zos, you will have the proxy and the contract in different directories.

Likely, you will use one of the proxy from https://github.com/zeppelinos/zos. Clone the `zos`, and install the dependencies:
```
git clone https://github.com/zeppelinos/zos
cd zos/packages/lib
npm install
rm contracts/mocks/WithConstructorImplementation.sol
```
Note:  `contracts/mocks/WithConstructorImplementation.sol` must be removed as it contains a [name clash collision](https://github.com/crytic/slither/wiki#keyerror-or-nonetype-error)  with `contracts/mocks/Invalid.sol`

Then from your project directory:
```
slither-check-upgradeability /path/to/zos/packages/lib/ UpgradeabilityProxy . ContractName
```

According to your setup, you might choose another proxy name than `UpgradeabilityProxy`.


## Checks

### Function ID checks

`slither-check-upgradeability` checks that:

* There is no function id collision between the proxy and the implementation
 * The proxy does not shadow any functions from the implementation

### Variable order checks

`slither-check-upgradeability` checks that:

* The variables are declared in the same order between the proxy and the implementation
* The variables are declared in the implementation first and second version

`slither-check-upgradeability` will warn if the proxy has state variables. Consider using the Unstructured storage pattern for those variables. 

### Initialization checks

Contracts based on `delegatecallproxy` cannot be initialized through constructors. As a result, init functions must be used. These functions do not benefit from:

* Solidity C3 linearization: an init function can be called multiple times, or never.
* The init functions must be calleable only once.
* Race conditions can occur during deployment.

If the contract uses the zos library (and the `Initializable` contract), the util checks that:

* All the `initialize` functions call the `initializer` modifier 
* All the inherits `initialize` functions are called
* `initialize` functions are never called twice

Additionally, `slither-check-upgradeability` prints the functions that must be called during initialization.

If the contract uses another schema, no check is performed. In that case, the tool warns:
```
Initializable contract not found, the contract does not follow a standard initialization schema.
```


