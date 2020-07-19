Configurations I've made on DL4 server for **Ubuntu 20.04**.

   - [`pyenv` virtual environment](#pyenv)
   - [](#)

---
## <a name="pyenv" />`pyenv` virtual environment
Links:
  - [Common build problems](https://github.com/pyenv/pyenv/wiki/common-build-problems)
  - [`pyenv` github](https://github.com/pyenv/pyenv)
  - [`pyenv` automatic installer](https://github.com/pyenv/pyenv-installer)
  - [Managing Multiple Python Versions With `pyenv`](https://realpython.com/intro-to-pyenv/)

```shell script
# Install required libraries
sudo apt install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git

sudo apt install libedit-dev

# The automatic installer
curl https://pyenv.run | bash

nano ~/.bashrc  # edit .bashrc file

# Load pyenv automatically by adding
# the following to the end of ~/.bashrc file:
export PATH="/home/lab225/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

exec $SHELL  # restart shell
pyenv update  # update
```

[Configure](https://realpython.com/intro-to-pyenv/)
Python virtual environment using `pyenv`.

```shell script
pyenv install --list  # show all available versions

# Choose Python version and install it.
# This will take a while because `pyenv`
# is building Python from source.
pyenv install -v 3.8.4

# Uninstall Python version if necessary
#rm -rf ~/.pyenv/versions/3.8.4
# or
#pyenv uninstall 3.8.4

# Set Python version
pyenv versions  # show installed versions
pyenv global 3.8.4  # use Python 3.8.4 globally
#pyenv local 3.8.4  # set an application-specific version
python --version  # get current Python version
#python -m test  # run the built-in test suite
#pyenv commands  # list of pyenv commands

# Create virtual environment
# pyenv virtualenv <python_version> <environment_name>
pyenv virtualenv 3.8.4 myproject

pyenv local myproject  # activate environment
#pyenv global myproject  # set global environment
pyenv deactivate  # deactivate environment
```

For example, install Anaconda:

```shell script
pyenv install anaconda3-2020.02

```