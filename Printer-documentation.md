Slither allows printing contracts information through its printers.

## Contract Summary
`slither file.sol --printers contract-summary`

Output a quick summary of the contract.
### Example
```
$ slither examples/printers/quick_summary.sol --printers contract-summary
```

<img src="https://raw.githubusercontent.com/trailofbits/slither/master/examples/printers/quick_summary.sol.png?sanitize=true">

## Function Summary
`slither file.sol --printers function-summary`

Output a summary of the contract showing for each function:
- What are the visibility and the modifiers 
- What are the state variables read or written
- What are the calls

### Example
```
$ slither tests/backdoor.sol --printers function-summary
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
`slither file.sol --printer inheritance-graph`

Output a graph showing the inheritance interaction between the contracts.

This printer requires xdot installed for vizualization:
```
sudo apt install xdot
```

### Example
```
$ slither examples/printers/inheritances.sol --printers inheritance-graph
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

<img src="https://raw.githubusercontent.com/trailofbits/slither/master/examples/printers/inheritances.sol.png?sanitize=true">

## Variables written and authorization
`slither file.sol --printers vars-and-auth`

Print the variables written and the check on `msg.sender` of each function.
### Example
```
...
$ slither examples/printers/authorization.sol --printers vars-and-auth
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
$ slither examples/printers/slihtir.sol --printers slithir
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
