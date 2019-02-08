Slither allows printing contracts information through its printers.

Num | Printer | Description
--- | --- | ---
1 | `call-graph` | Export the call-graph of the contracts to a dot file
2 | `cfg` | Export the CFG of each functions
3 | `contract-summary` | Print a summary of the contracts
4 | `function-id` | Print the keccack256 signature of the functions
5 | `function-summary` | Print a summary of the functions
6 | `human-summary` | Print a human readable summary of the contracts
7 | `inheritance` | Print the inheritance relations between contracts
8 | `inheritance-graph` | Export the inheritance graph of each contract to a dot file
9 | `slithir` | Print the slithIR representation of the functions
10 | `slithir-ssa` | Print the slithIR representation of the functions
11 | `variables-order` | Print the storage order of the state variables
12 | `vars-and-auth` | Print the state variables written and the authorization of the functions

## Call Graph
`slither file.sol --print call-graph`

Export the call-graph of the contracts to a dot file
### Example
```
$ slither examples/printers/call_graph.sol --printers contract-summary
```
<img src="https://raw.githubusercontent.com/trailofbits/slither/master/examples/printers/call_graph.sol.dot.png?sanitize=true">

The output format is [dot](https://www.graphviz.org/).
To vizualize the graph:
```
xdot examples/printers/call_graph.sol.dot
```
To convert the file to svg:
```
dot examples/printers/call_graph.sol.dot -Tsvg -o examples/printers/call_graph.sol.png
```


## CFG
Export the control flow graph of each functions

`slither file.sol --print cfg`

### Example

The output format is [dot](https://www.graphviz.org/).
To vizualize the graph:
```
xdot function.sol.dot
```
To convert the file to svg:
```
dot function.dot -Tsvg -o function.sol.png
```


## Contract Summary
`slither file.sol --print contract-summary`


Output a quick summary of the contract.
### Example
```
$ slither examples/printers/quick_summary.sol --printers contract-summary
```

<img src="https://raw.githubusercontent.com/trailofbits/slither/master/examples/printers/quick_summary.sol.png?sanitize=true">

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

## Inheritance Graph
`slither file.sol --print inheritance-graph`

Output a graph showing the inheritance interaction between the contracts.

This printer requires xdot installed for vizualization:
```
sudo apt install xdot
```

### Example
```
$ slither examples/printers/inheritances.sol --print inheritance-graph
[...]
INFO:PrinterInheritance:Inheritance Graph: examples/DAO.sol.dot
```

The output format is [dot](https://www.graphviz.org/).
To vizualize the graph:
```
xdot examples/printers/inheritances.sol.dot
```
To convert the file to svg:
```
dot examples/printers/inheritances.sol.dot -Tsvg -o examples/printers/inheritances.sol.png
```

Functions in orange override a parent's functions. If a variable points to another contract, the contract type is written in blue.

<img src="https://raw.githubusercontent.com/trailofbits/slither/master/examples/printers/inheritances_graph.sol.png?sanitize=true">

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

## SlithIR
`slither file.sol --printers slithir`

Print the IR for every function

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
