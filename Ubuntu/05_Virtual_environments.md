   - [Task](#task)
   - [Anaconda virtual environment](#anaconda)
      - [Install Anaconda](#install-anaconda)
      - [Set up and configure Anaconda virtual environment](#configure-anaconda)
   - [Common virtual environment](#venv)
      - [Install packages for virtual environment](#install-packages)
      - [Set up and configure virtual environment using `virtualenvwrapper`](#configure-venv)
   - [EasyBuild environment on SURFsara server](#easy-build)

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

#### <a name="install-anaconda" />Install Anaconda

```shell
# Download latest Anaconda distribution
mkdir -p ~/Documents/Install/Anaconda/
cd ~/Documents/Install/Anaconda
curl -O https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh

# Run installation script
bash Anaconda3-2019.10-Linux-x86_64.sh

# Next, you will be prompted to download Visual Studio Code,
# which you can learn more about from the official VSCode website.
# Type 'no' to decline, if you're not sudo user.

# Activate Installation
source ~/.bashrc
# Test installation
conda list
```

#### <a name="configure-anaconda" />Set up and configure Anaconda virtual environment

```shell
# To activate installed Anaconda use command:
source ~/.bashrc

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
python -c "import tensorflow as tf;     \
    print('Version:', tf.__version__);  \
    tf.enable_eager_execution();        \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"

# Activate and deactivate virtual environment
conda deactivate  # exit to the "base" environment
conda deactivate  # exit from Anaconda base env
conda activate myenv

# Install necessary packages for Ubuntu 18.04 in one line:
conda install tensorflow-gpu matplotlib scipy opencv pillow scikit-learn \
              scikit-image pandas ipython ipyparallel jupyter pyyaml -n myenv

# Or in many lines. Note: now Keras is a part of TensorFlow package.
conda install -c michael_wild opencv-contrib  # use 'opencv' for Ubuntu
conda install -c anaconda pillow  # use 'pillow' for Ubuntu
conda install scikit-learn
conda install scikit-image
conda install scipy
conda install matplotlib
conda install pandas
conda install ipython
conda install jupyter
conda install ipyparallel  # ipyparallel is needed for jupyter
conda install pyyaml  # required to save models in YAML format

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

#### <a name="install-packages" />Install packages for virtual environment

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

How-to install `virtualenvwrapper` [for Windows 10](https://pypi.org/project/virtualenvwrapper-win):

```shell
rem For TensorFlow you have to install
rem Python >= 3.7.2 (64-bit); CUDA >= 10.0; cuDNN >= 7.5
:
set PATH=c:\path-to-your\Python37\Scripts\;c:\path-to-your\Python37\;%PATH%
: In my case: set PATH=c:\Programs\Python37\Scripts\;c:\Programs\Python37\;%PATH%
echo %PATH%
pip3 --version
pip3 install virtualenvwrapper-win
```

User can install packages locally, but it is better to use virtual environment.

```shell
# user installation
# pip install --user packagename

# Install Pipenv
pip install --user pipenv
```

#### <a name="configure-venv" />Set up and configure virtual environment using `virtualenvwrapper`

```shell
# Check version of virtual environment
virtualenv --version
# We'll use virtualenvwrapper — is a set of extensions to Ian Bicking's virtualenv tool.
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

# Install TensorFlow 2.0 alpha0 — make sure you need this version of TF.
pip install -U --pre tensorflow-gpu
# or
pip install tensorflow-gpu==2.0.0-alpha0 

# Check it
python -c "import tensorflow as tf;     \
    print('Version:', tf.__version__);  \
    print(tf.reduce_sum(tf.random.normal([1000, 1000])));"

# Install all other packages into myenv
pip install tensorflow-gpu matplotlib scipy opencv-contrib-python Pillow \
            scikit-learn scikit-image pandas ipython ipyparallel jupyter pyyaml

# Install PyTorch if necessary
# NOTE: check your installation here: https://pytorch.org/
pip install torch torchvision

# Deactivate myenv
deactivate

# Delete virtual environment if you don't need it any more.
rmvirtualenv myenv
```

---
### <a name="easy-build" />EasyBuild environment on SURFsara server

Links to read:
   * [Tutorial: Easybuild and Environment Modules](https://varrette.gforge.uni.lu/blog/2017/06/01/tutorial-easybuild/)
   * [EasyBuild documentation](https://easybuild.readthedocs.io/en/latest)

Notes:
   - You can login to SURFsara only from DeepLab3, because its IP address is in the whitelist.
   - [SURFsara documentation page](https://userinfo.surfsara.nl/systems/cartesius/usage/batch-usage) --
   You can try to use the search functionality of userinfo as not everything is referenced, but is findable.
   - Use `accinfo` command or [`portal.surfsara.nl`](https://portal.surfsara.nl) to track hours remained.
   - If you have problems interacting with the batch environment please send a message at
   `helpdesk <at> surfsara <dot> nl`.
   - If you have any ML framework/application setup, cluster behavior, etc. you can email me directly
   `"Damian Podareanu" <damian <dot> podareanu <at> surfsara <dot> nl>` and just CC
   `helpdesk <at> surfsara <dot> nl`.
   - `nvidia-smi` command doesn't work.
   - Operating system: Red Hat Enterprise Linux Server 7.6 (Maipo). Command to check: `hostnamectl`
   - 17 TB of free space in $HOME directory. Command to check: `df -h ~/`
   - 16 CPU cores and 126 GB RAM. Command to check: `htop`

You can install local software modules in EasyBuild environment
or use [Anaconda virtual environment](#anaconda) from previous chapter.

```shell
module --help    # Get help about Modules package
man module       # Get manual about Modules package
module avail     # List all the modules which are available to be loaded.
module list      # List loaded modules

module avail | grep -i conda  # search for 'conda' packages
module av | grep -i python    # search for 'python' packages
module av python              # search for 'python' - case sensitive
module available Python       # search for 'Python' - case sensitive

module load eb   # Load EasyBuild framework
eb -S Anaconda   # List Anaconda packages
eb -S MiniConda  # List Miniconda packages
```

Install and configure Anaconda:

```shell
# Install Anaconda package. Wait for 5 minutes.
module load eb   # Load EasyBuild framework
eblocalinstall Anaconda3-5.3.0.eb --robot

# Load Anaconda module. To load Anaconda 5.3.0 you must exit and login again.
# Otherwise you can load only older version Anaconda 5.0.1.
exit  # exit from SURFsara and login again to activate Anaconda3 5.3.0
module load Anaconda3/5.3.0
#module load Anaconda3/5.0.1

# Check which Anaconda is used
which anaconda
conda --version
# Set up 'test' Anaconda virtual environment
conda create --name test python=3
# Show virtual environments
conda info --envs
# Activate 'test' environment
source activate test

# Install necessary packages with one line in activated 'test' environment
conda install tensorflow-gpu matplotlib scipy opencv pillow scikit-learn \
              scikit-image pandas ipython ipyparallel jupyter pyyaml -n test

# Update Anaconda if necessary
conda update -n base -c defaults conda
# If you have inconsistency problem, run
conda install anaconda

# Deactivate 'test' environment
source deactivate

# Delete 'test' vitrual environment if necessary
source deactivate
conda remove --name test --all
conda info --envs
```

Install and configure Python:

```shell
# There are several Python versions installed already
module av Python
# Load Python module
module load Python/3.6.3-foss-2017b
# Load cuDNN module, so TensorFlow can be imported
module load cuDNN/7.3.1-CUDA-10.0.130
# Create 'test' virtual environment
virtualenv test
# Activate 'test' virtual environment
source ~/test/bin/activate
# Check which Python is used
which python
python --version
# Start Python in the local environment
python3

# Install necessary packages with one line in activated 'test' environment
pip install tensorflow-gpu matplotlib scipy opencv-contrib-python Pillow \
            scikit-learn scikit-image pandas ipython ipyparallel jupyter pyyaml

# Deactivate 'test' environment
deactivate

# Delete 'test' virtual environment if you don't need it any more.
rmvirtualenv test
```

To transfer files via `gsiscp` and `gsisftp` read
[this instruction](14_Install_GSI_openssh_client.md/#manage).
