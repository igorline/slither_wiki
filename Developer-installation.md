To use a developer version of slither (up-to-date with https://github.com/trailofbits/slither), we recommend to use python virtualenv:
```
pip3 install virtualenvwrapper
source /usr/local/bin/virtualenvwrapper.sh
mkvirtualenv --python=`which python3` slither-dev
git clone https://github.com/trailofbits/slither
cd slither
python setup.py develop
```

Then on any terminal, run
```
workon slither-dev
```
To have a shell with a virtual python environment where slither is installed.

To update slither, simply run `git pull` from the slither directory.