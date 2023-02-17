## Developer Installation using pyenv

Install [pyenv](https://github.com/pyenv/pyenv#installation) and [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv#installing-with-homebrew-for-macos-users) if you don't have them already.

```
git clone https://github.com/trailofbits/slither
cd slither
pyenv virtualenv slitherdev
echo "slitherdev" > .python-version
```

Make sure the slitherdev environment is activated using `pyenv virtualenvs`

Now run `pip install -e ".[dev]"`

Slither is now installed to the local `slitherdev` environment. 

### Setting up IDE-based debugging

1. Configure your IDE to use `$(pyenv root)/slitherdev/bin/python` as the interpreter.
2. Use `slither` as the entrypoint for the debugger.
3. Pycharm specific: Set the environment working directory to `$(pyenv root)/versions/slitherdev/bin/`

## Developer Installation using Virtualenvs 

Use [virtualenv](https://virtualenvwrapper.readthedocs.io/en/latest/) to install a developer version of Slither (up-to-date with `master`):
```
pip3 install virtualenvwrapper
source /usr/local/bin/virtualenvwrapper.sh
mkvirtualenv --python=`which python3` slither-dev
git clone https://github.com/trailofbits/slither
cd slither
pip install -e ".[dev]"
```

Start a shell with the Slither virtual environment by running:
```
workon slither-dev
```

Update Slither by running `git pull` from the slither directory.