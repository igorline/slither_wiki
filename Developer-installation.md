## Developer Installation

Slither currently runs requires at least Python3.8 so make sure you have a sufficiently up-to-date installation by running `python --version`.
We recommend [pyenv](https://github.com/pyenv/pyenv) to manage python versions.

To start working on modifications to Slither locally, run:
```shell
git clone https://github.com/crytic/slither
cd slither
git checkout dev
make dev
```
This will create a virtual environment, `./env/`, in the root of the repo. 

To run commands using your development version of Slither, run:
```shell
source ./env/bin/activate
```

### Setting up IDE-based debugging

1. Configure your IDE to use `./env/bin/python` as the interpreter.
2. Use `slither` as the entrypoint for the debugger.
3. Pycharm specific: Set the environment working directory to `./env/bin/`