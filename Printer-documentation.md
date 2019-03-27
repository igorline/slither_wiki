Slither allows printing contracts information through its printers.

Num | Printer | Description
--- | --- | ---
1 | `call-graph` | [Export the call-graph of the contracts to a dot file](https://github.com/crytic/slither/wiki/Printer-documentation#call-graph)
2 | `cfg` | [Export the CFG of each functions](https://github.com/crytic/slither/wiki/Printer-documentation#cfg)
3 | `contract-summary` | [Print a summary of the contracts](https://github.com/crytic/slither/wiki/Printer-documentation#contract-summary)
4 | `data-dependency` | [Print the data dependencies of the variables](https://github.com/crytic/slither/wiki/Printer-documentation#data-dependencies)
5 | `function-id` | [Print the keccack256 signature of the functions](https://github.com/crytic/slither/wiki/Printer-documentation#function-id)
6 | `function-summary` | [Print a summary of the functions](https://github.com/crytic/slither/wiki/Printer-documentation#function-summary)
7 | `human-summary` | [Print a human-readable summary of the contracts](https://github.com/crytic/slither/wiki/Printer-documentation#human-summary)
8 | `inheritance` | [Print the inheritance relations between contracts](https://github.com/crytic/slither/wiki/Printer-documentation#inheritance)
9 | `inheritance-graph` | [Export the inheritance graph of each contract to a dot file](https://github.com/crytic/slither/wiki/Printer-documentation#inheritance-graph)
10 | `modifiers` | [Print the modifiers called by each function](https://github.com/crytic/slither/wiki/Printer-documentation#modifiers)
11 | `require` | [Print the require and assert calls of each function](https://github.com/crytic/slither/wiki/Printer-documentation#require)
12 | `slithir` | [Print the slithIR representation of the functions](https://github.com/crytic/slither/wiki/Printer-documentation#slithir)
13 | `slithir-ssa` | [Print the slithIR representation of the functions](https://github.com/crytic/slither/wiki/Printer-documentation#slithir-ssa)
14 | `variables-order` | [Print the storage order of the state variables](https://github.com/crytic/slither/wiki/Printer-documentation#variables-order)
15 | `vars-and-auth` | [Print the state variables written and the authorization of the functions](https://github.com/crytic/slither/wiki/Printer-documentation#variables-written-and-authorization)


Several printers require xdot installed for visualization:
```
sudo apt install xdot
```

## Call Graph
`slither file.sol --print call-graph`

Export the call-graph of the contracts to a dot file
### Example
```
$ slither examples/printers/call_graph.sol --print contract-summary
```
<img src="https://raw.githubusercontent.com/crytic/slither/master/examples/printers/call_graph.sol.dot.png?sanitize=true">

The output format is [dot](https://www.graphviz.org/).
To vizualize the graph:
```
$ xdot examples/printers/call_graph.sol.dot
```
To convert the file to svg:
```
$ dot examples/printers/call_graph.sol.dot -Tpng -o examples/printers/call_graph.sol.png
```


## CFG
Export the control flow graph of each function

`slither file.sol --print cfg`

### Example

The output format is [dot](https://www.graphviz.org/).
To vizualize the graph:
```
$ xdot function.sol.dot
```
To convert the file to svg:
```
$ dot function.dot -Tsvg -o function.sol.png
```


## Contract Summary

Output a quick summary of the contract.

`slither file.sol --print contract-summary`

### Example
```
$ slither examples/printers/quick_summary.sol --print contract-summary
```

<img src="https://raw.githubusercontent.com/crytic/slither/master/examples/printers/quick_summary.sol.png?sanitize=true">


## Data Dependencies

Print the data dependencies of the variables
`slither file.sol --print data-dependency`

### Example
```
$ slither examples/printers/data_dependencies.sol --print data-dependency
```
```
Contract MyContract
+----------+----------------------+
| Variable |     Dependencies     |
+----------+----------------------+
|    a     |     ['input_a']      |
|    b     | ['input_b', 'input'] |
|    c     |          []          |
+----------+----------------------+

Function setA(uint256,uint256)
+--------------+--------------+
|   Variable   | Dependencies |
+--------------+--------------+
|   input_a    |      []      |
|   input_b    |      []      |
| MyContract:a | ['input_a']  |
| MyContract:b |      []      |
| MyContract:c |      []      |
+--------------+--------------+
Function setB(uint256)
+--------------+----------------------+
|   Variable   |     Dependencies     |
+--------------+----------------------+
|    input     |     ['input_b']      |
| MyContract:a |          []          |
| MyContract:b | ['input_b', 'input'] |
| MyContract:c |          []          |
+--------------+----------------------+
```

## Function id
`slither file.sol --print function-id`
Print the keccack256 signature of the functions

### Examples
```
$ slither examples/printers/authorization.sol --print function-id
INFO:Printers:
MyContract:
+---------------+------------+
|      Name     |     ID     |
+---------------+------------+
| constructor() | 0x90fa17bb |
| mint(uint256) | 0xa0712d68 |
+---------------+------------+
```

## Function Summary
`slither file.sol --print function-summary`

Output a summary of the contract showing for each function:
- What are the visibility and the modifiers 
- What are the state variables read or written
- What are the calls

### Example
```
$ slither tests/backdoor.sol --print function-summary
```
```
[...]

Contract C
Contract vars: []
Inheritances:: []
 
+-----------------+------------+-----------+----------------+-------+---------------------------+----------------+
|     Function    | Visibility | Modifiers |      Read      | Write |       Internal Calls      | External Calls |
+-----------------+------------+-----------+----------------+-------+---------------------------+----------------+
| i_am_a_backdoor |   public   |     []    | ['msg.sender'] |   []  | ['selfdestruct(address)'] |       []       |
+-----------------+------------+-----------+----------------+-------+---------------------------+----------------+

+-----------+------------+------+-------+----------------+----------------+
| Modifiers | Visibility | Read | Write | Internal Calls | External Calls |
+-----------+------------+------+-------+----------------+----------------+
+-----------+------------+------+-------+----------------+----------------+
```

## Human Summary
`slither file.sol --print contract-summary`

Print a human-readable summary of the contracts

### Example
```
$ slither examples/printers/human_printer.sol --print human-summary
```

<img src="https://raw.githubusercontent.com/crytic/slither/master/examples/printers/human_printer.sol.png?sanitize=true">

## Inheritance
`slither file.sol --print inheritance`
Print the inheritance relations between contracts

### Example
```
$ slither examples/printers/inheritances.sol --print inheritance
```

<img src="https://raw.githubusercontent.com/crytic/slither/master/examples/printers/inheritances.sol.png?sanitize=true">

## Inheritance Graph
`slither file.sol --print inheritance-graph`

Output a graph showing the inheritance interaction between the contracts.



### Example
```
$ slither examples/printers/inheritances.sol --print inheritance-graph
[...]
INFO:PrinterInheritance:Inheritance Graph: examples/DAO.sol.dot
```

The output format is [dot](https://www.graphviz.org/).
To vizualize the graph:
```
$ xdot examples/printers/inheritances.sol.dot
```
To convert the file to svg:
```
$ dot examples/printers/inheritances.sol.dot -Tsvg -o examples/printers/inheritances.sol.png
```
<img src="https://raw.githubusercontent.com/crytic/slither/master/examples/printers/inheritances_graph.sol.png?sanitize=true">

Indicators:
- If a contract has multiple inheritance, the connecting edges will be labelled in order of declaration.
- Functions highlighted orange override a parent's function.
- Functions which do not override each other directly (but collide due to multiple inheritance) will be emphasized at the bottom of the affected contract node in grey font.
- Variables highlighted red overshadow a parent's variable declaration.
- Variables of type `contract` specify the contract name in parentheses in a blue font.


## Modifiers
`slither file.sol --print modifiers`

Print the modifiers called by each function.

### Example
```
$ slither examples/printers/modifier.sol --print modifiers
INFO:Printers:
Contract C
+-------------+-------------+
|   Function  |  Modifiers  |
+-------------+-------------+
| constructor |      []     |
|     set     | ['isOwner'] |
+-------------+-------------+
```

## Require
`slither file.sol --print require`

Print the require and assert calls of each function.

### Example
```
$ slither examples/printers/require.sol --print require
INFO:Printers:
Contract Lib
+----------+--------------------------------------+
| Function |          require or assert           |
+----------+--------------------------------------+
|   set    | require(bool)(msg.sender == s.owner) |
+----------+--------------------------------------+
INFO:Printers:
Contract C
+-------------+--------------------------------------+
|   Function  |          require or assert           |
+-------------+--------------------------------------+
| constructor |                                      |
|     set     | require(bool)(msg.sender == s.owner) |
+-------------+--------------------------------------+
```



## SlithIR
`slither file.sol --print slithir`

Print the slithIR representation of the functions

### Example
```
$ slither examples/printers/slihtir.sol --print slithir
Contract UnsafeMath
	Function add(uint256,uint256)
		Expression: a + b
		IRs:
			TMP_0(uint256) = a + b
			RETURN TMP_0
	Function min(uint256,uint256)
		Expression: a - b
		IRs:
			TMP_0(uint256) = a - b
			RETURN TMP_0
Contract MyContract
	Function transfer(address,uint256)
		Expression: balances[msg.sender] = balances[msg.sender].min(val)
		IRs:
			REF_3(uint256) -> balances[msg.sender]
			REF_1(uint256) -> balances[msg.sender]
			TMP_1(uint256) = LIBRARY_CALL, dest:UnsafeMath, function:min, arguments:['REF_1', 'val'] 
			REF_3 := TMP_1
		Expression: balances[to] = balances[to].add(val)
		IRs:
			REF_3(uint256) -> balances[to]
			REF_1(uint256) -> balances[to]
			TMP_1(uint256) = LIBRARY_CALL, dest:UnsafeMath, function:add, arguments:['REF_1', 'val'] 
			REF_3 := TMP_1
```

## SlithIR-SSA
`slither file.sol --print slithir-ssa`

Print the slithIR representation of the functions (SSA version)

## Variable order
`slither file.sol --print variable-order`

Print the storage order of the state variables

### Example
```
$ slither tests/check-upgradability/contractV2_bug.sol --print variable-order
INFO:Printers:
ContractV2:
+-------------+---------+
|     Name    |   Type  |
+-------------+---------+
| destination | uint256 |
|    myFunc   | uint256 |
+-------------+---------+

```


## Variables written and authorization
`slither file.sol --print vars-and-auth`

Print the variables written and the check on `msg.sender` of each function.
### Example
```
...
$ slither examples/printers/authorization.sol --print vars-and-auth
[..]
INFO:Printers:
Contract MyContract
+-------------+-------------------------+----------------------------------------+
|   Function  | State variables written |        Conditions on msg.sender        |
+-------------+-------------------------+----------------------------------------+
| constructor |        ['owner']        |                   []                   |
|     mint    |       ['balances']      | ['require(bool)(msg.sender == owner)'] |
+-------------+-------------------------+----------------------------------------+
```