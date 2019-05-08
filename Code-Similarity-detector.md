# Code Similarity Detector

`slither-simil` is a tool designed to use state-of-the-art Machine Learning to detect similar Solidity functions.
Our tool should be trained with a large corpus of contracts to work. We offer a pretrained model from  
[etherscan_verified_contracts](https://github.com/thec00n/etherscan_verified_contracts) with 60,000 contracts counting more than 850,000 functions.
`slither-simil` uses a vector embedding technique called [FastText](https://github.com/facebookresearch/fastText) to generate a compact numerical representation of every function. We use this library because:
* it implements several state-of-the-art techniques such as nbow and skipgrams,
* it has high performance (it is C++ code with Python bindings),
* it has MIT license,
* it is well maintained (by Facebook).
 
## Requierements

Before start using `slither-simil`, install the required packages:

```
$ pip3 install pybind11 --user
$ pip3 install https://github.com/facebookresearch/fastText/archive/0.2.0.zip --user
$ pip3 install matplotlib --user
```

Make sure that you are using pip3.6 or later. If you are running from inside a [virtualenv](https://virtualenv.pypa.io/en/latest/), remove the `--user` parameter.

## Usage

All these examples will use the following files:

* [etherscan_verified_contracts.bin](https://drive.google.com/file/d/1oEhbIL4V9582Y5VKp4iiOURGq8qa4cBN/view?usp=sharing)
* [cache.npz](https://drive.google.com/file/d/1vpwusbyzLn1JqqAvlFivHXtLvsEp0VqX/view?usp=sharing)
* [MetaCoin.sol](link)

Our tool has several modes:
- `info`: prints the raw representation of a function and its information about a function

### Info mode

This mode has two features. You can either inspect the internal information about a pre-trained model:

```
$ slither-simil info etherscan_verified_contracts.bin 
etherscan_verified_contracts.bin uses the following words:
</s>
index(uint256)
return
condition(temporary_variable)
member
solidity_call(require(bool))
library_call
binary(+)
event
(local_solc_variable(default)):=(temporary_variable)
binary(==)
...
```

or examine the internal representation of function:

```
$ slither-simil info etherscan_verified_contracts.bin --filename MetaCoin.sol --contract MetaCoin --fname sendCoin
Function sendCoin in contract MetaCoin is encoded as:
index(uint256) binary(<) condition(temporary_variable) return index(uint256) binary(-) index(uint256) binary(+) event return
[ 0.00689753 -0.05349572 -0.06854086 -0.01667773  0.1259813  -0.05974023
  0.06719872 -0.04520541  0.13745852  0.14690697 -0.03721125  0.00579037
...
```

### Test mode

...

### Train mode

...

### Plot mode

...