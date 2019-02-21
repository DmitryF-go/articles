   - [Task](#task)
   - [Installation](#installation)
   - [Use virtual environment](#venv)
   - [Anaconda virtual environment](#anaconda)

---
### <a name="task" />Task

Install software for virtual environments.

---
### <a name="installation" />Installation

```shell
# Python virtual environment creator
sudo apt install virtualenv
sudo apt install python-virtualenv
sudo apt install python3-virtualenv

# Library for generating Python executable zip files
sudo apt install pex
sudo apt install python-pex
sudo apt install python3-pex

# Node.js virtual environment builder
sudo apt install nodeenv

# System for automatically handling virtual environments
sudo apt install fades

# Wrap and build python packages using virtualenv
sudo apt install dh-virtualenv

# Script for cloning a non-relocatable virtualenv
sudo apt install virtualenv-clone

# Extension to virtualenv for managing multiple virtual Python environments
sudo apt install virtualenvwrapper

# apt virtual environment
sudo apt install apt-venv

# pyvenv-3 binary for python 3.x
sudo apt install python3-venv
```

---
### <a name="venv" />Use virtual environment

[Pipenv & Virtual Environments](https://docs.python-guide.org/dev/virtualenvs)

```shell
# user installation
# pip install --user packagename

# Install Pipenv
pip install --user pipenv

# Check version of virtual environment
virtualenv --version

# Create virtual environment
virtualenv -p python3.6 venv3.6

# Activate virtual environment
source venv3.6/bin/activate

# Install "requests" package into venv3.6
pip install requests
pip list  # check it

# Deactivate venv3.6
deactivate
```

---
### <a name="anaconda" />Anaconda virtual environment

See how-to install Anaconda and use its virtual environment
[here](06_Various_software_install.md#anaconda).
