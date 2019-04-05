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

`proxy.sol`/`implem.sol` can be a Truffle directory. If your proxy and your implementation are in different directories and the solc compilation is complex, you can first compile the contract to its ast compact json output (`solc file.sol --ast-compact-json > file.json`), and use the generated json file (instead of `file.sol`).

If a second version of the contract is to be checked, use:

```
slither-check-upgradability proxy.sol ProxyName implem.sol ContractName implem_v2.sol ContractNameV2
```


## Initialization checks

Contracts based on `delegatecallproxy` cannot be initialized through constructors. As a result, init functions must be used. These functions do not benefitfrom:
- The Solidity C3 linearization. As a result, an init function can be called multiple times, or never.
- The init functions must be calleable only once.
- Race conditions can occur during deployement.

If the contract uses the zos library (and the `Initializable` contract), the util checks that:
 - All the `initalize` functions call the `initializer` modifier 
 - All the inherits `initalize` functions are called
 - `initalize` functions are never called twice

If the contract uses another schema, no check is performed. In that case, the tool warns:
```
Initializable contract not found, the contract does not follow a standard initalization schema.
```

## Functions ids checks

<TODO>

## Variables order checks

<TODO>