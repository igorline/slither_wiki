Use [virtualenv](https://virtualenvwrapper.readthedocs.io/en/latest/) to install a developer version of Slither (up-to-date with `master`):
```
pip3 install virtualenvwrapper
source /usr/local/bin/virtualenvwrapper.sh
mkvirtualenv --python=`which python3` slither-dev
git clone https://github.com/trailofbits/slither
cd slither
python setup.py develop
```

Start a shell with the Slither virtual environment by running:
```
workon slither-dev
```

Update Slither by running `git pull` from the slither directory.