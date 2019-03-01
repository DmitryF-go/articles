   - [Task](#task)
   - [Installation](#installation)
   - [Anaconda virtual environment](#anaconda)
   - [Common virtual environment](#venv)

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
### <a name="anaconda" />Anaconda virtual environment

I recommend Anaconda virtual environment, but you could use common virtual env
described in the next chapter.

Links:
   - [How To Install Anaconda on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart)
   - [Anaconda distribution](https://www.anaconda.com/distribution)
   - [Anaconda repository](https://repo.anaconda.com/archive)

```shell
# Download latest Anaconda distribution
mkdir -p ~/Documents/Install/Anaconda/
cd ~/Documents/Install/Anaconda
curl -O https://repo.anaconda.com/archive/Anaconda3-2018.12-Linux-x86_64.sh

# Run installation script
bash Anaconda3-2018.12-Linux-x86_64.sh

# Next, you will be prompted to download Visual Studio Code,
# which you can learn more about from the official VSCode website.
# Type 'no' to decline, if you're not sudo user.

# Activate Installation
source ~/.bashrc
# Test installation
conda list

# Set up Anaconda virtual environment
conda create --name myenv python=3
# Show virtual envs
conda info --envs
# Activate the new environment
conda activate myenv

# After activation of virtual environment,
# install TensorFlow using Anaconda
conda install tensorflow-gpu
# Check it
python -c "import tensorflow as tf;  \
    tf.enable_eager_execution();      \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"

# Activate and deactivate virtual environment
conda deactivate  # exit to the "base" environment
conda deactivate  # exit from Anaconda base env
conda activate myenv

# Install necessary modules into virtual environment
conda install opencv
conda install -c anaconda pillow
conda install scikit-learn
conda install scipy
conda install matplotlib
conda install pandas
conda install keras
conda install ipython
conda install jupyter
conda install ipyparallel  # needed for jupyter

# Delete vitrual environment
conda remove --name myenv --all
conda info --envs
```

---
### <a name="venv" />Common virtual environment

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
