   - [Task](#task)
   - [Anaconda virtual environment](#anaconda)
   - [Common virtual environment](#venv)

---
### <a name="task" />Task

Install software for virtual environments. Set up and configure virtual env.

---
### <a name="anaconda" />Anaconda virtual environment

I recommend Anaconda virtual environment, but you could use common virtual env
described in the next chapter.

Links:
   - [How To Install Anaconda on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart)
   - [Anaconda distribution](https://www.anaconda.com/distribution)
   - [Anaconda repository](https://repo.anaconda.com/archive)

Install Anaconda:

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
```

Set up and configure Anaconda virtual environment:

```shell
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

# Install necessary packages into virtual environment
# Now Keras is a part of TensorFlow package.
conda install opencv
conda install -c anaconda pillow
conda install scikit-learn
conda install scikit-image
conda install scipy
conda install matplotlib
conda install pandas
conda install ipython
conda install jupyter
# ipyparallel is needed for jupyter
conda install ipyparallel

# Or install the whole bunch of packages in one line
conda install tensorflow-gpu opencv pillow scikit-learn scikit-image pandas ipython ipyparallel jupyter -n myenv

# Delete vitrual environment
conda deactivate
conda remove --name myenv --all
conda info --envs
```

---
### <a name="venv" />Common virtual environment

Links:
   - [Python Virtual Environment](https://www.geeksforgeeks.org/python-virtual-environment)
   - [Pipenv & Virtual Environments](https://docs.python-guide.org/dev/virtualenvs)

Install packages for virtual environment:

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

User can install packages locally, but it is better to use virtual environment.

```shell
# user installation
# pip install --user packagename

# Install Pipenv
pip install --user pipenv
```

Set up and configure virtual environment:

```shell
# Check version of virtual environment
virtualenv --version
# We'll use virtualenvwrapper -- is a set of extensions to Ian Bicking's virtualenv tool.
virtualenvwrapper

# Create virtual environment for Python 3.x
mkvirtualenv -p /usr/bin/python3 myenv

# Directory called "myenv" is created
ls -hal ~/.virtualenvs/myenv/
# Browse inside of "myenv" dir to view its structure.

# List all virtual environments
lsvirtualenv -b  # breaf mode
# or
lsvirtualenv -l  # long mode

# Activate virtual environment
workon myenv

# You should see "(myenv)" before command prompt
# (myenv) username@hostname:~/path/to/dir$
# Make sure you "workon myenv" and install packages into myenv

# Install TensorFlow 2.0 alpha0 -- make sure you need this version of TF.
pip install -U --pre tensorflow-gpu
# or
pip install tensorflow-gpu==2.0.0-alpha0 

# Check it
python -c "import tensorflow as tf;     \
    print('Version:', tf.__version__);  \
    print(tf.reduce_sum(tf.random.normal([1000, 1000])));"

# Install all other packages into myenv
pip install opencv-contrib-python Pillow scikit-learn scikit-image pandas ipython ipyparallel jupyter

# Deactivate myenv
deactivate

# Delete virtual environment if you don't need it any more.
rmvirtualenv myenv
```
