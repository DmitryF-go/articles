   - [Task](#task)
   - [`apt` versus `pip` installer](#versus)
   - [Python 2.x and 3.x installation](#python)
   - [Install additional packages](#packages)
   - [Check installations](#check)
   - [Search and remove packages](#additional)

---
### <a name="task" />Task

Install Python and all needed packages on Ubuntu 18.04.

My answer on AskUbuntu.com: https://askubuntu.com/a/1097135/672237

---
### <a name="versus" />`apt` versus `pip` installer

It is better use `apt` (`apt-get`) command instead of `pip` command, because:
   1. `apt` installs only tested on Ubuntu packages and depencences;
   2. `sudo apt` `update` / `upgrage` command keeps packages up to date;
   3. if you want install / update packages for all users on Ubuntu system,
      not only for your own local account;
   4. if you want packages for Ubuntu, so operating system could use them too.
https://askubuntu.com/a/1097211/672237
For other versions of packages one should use virtual environment.
See [virtual environment installation instruction](05_Virtual_environments.md).
Or build and test packages from the source codes (_for specialists only_).

---
### <a name="python" />Python 2.x and 3.x installation

Update Ubuntu before every significant software installation.

```shell
# Refresh repositories
sudo apt update
# Update software
sudo apt upgrade
```

Install Pip and then install Python.

```shell
# install pip for 2.x
sudo apt install python-pip
# Install python 2.7 version
sudo apt install python2.7

# install pip for 3.x
sudo apt install python3-pip
# Install currently supported by Ubuntu python 3.x version.
sudo apt install python3
```

:exclamation: **Don't delete current python3,
otherwise Ubuntu OS will _BROKE_.** :exclamation:

<details close>
  <summary>Better don't install the newest versions 3.7, 3.8, 4.0, etc.
  globally (on the whole OS).</summary>

This command works:

```shell
# Bad idea to install 3.7 while 3.6 is the current vertion.
sudo apt install python3.7  # bad idea
```

But it's a bad idea to use several versions globally, because in this case
import of NumPy (`import numpy`) and other modules will fail for python 3.7.

Currently installed NumPy is for 3.6 (global) python version for Ubuntu,
not 3.7. So it'll fail to import in the 3.7 version when you'll try.

```shell
# For example:
sudo apt install python3.7  # bad idea - 3.6 and 3.7 at the same time
sudo apt install python3-numpy  # install NumPy for 3.6, not 3.7
python3.7  # call 3.7
import numpy
# Error!
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/numpy/core/__init__.py", line 16, in <module>
    from . import multiarray
ImportError: cannot import name 'multiarray' from 'numpy.core'
(/usr/lib/python3/dist-packages/numpy/core/__init__.py)

# Remove python 3.7
sudo apt remove python3.7
sudo apt autoremove
```

Use `sudo apt install python3` not `sudo apt install python3.7` command
for python 3.x installation. If you need 3.7 or newer, use
local virtual environment. It's a bad idea to have several versions of
python 3.x globally at the same time. Use only currently supported
by Ubuntu python 3.x version globally. At this moment it is 3.6.

</details>
<br/>

---
### <a name="packages" />Install additional packages

Install additional packages. Choose which package you need for work.
Here are most common packages from everyday usage. However you could
have another list.

```shell
# Install packages:
sudo apt install python-numpy  # numpy
sudo apt install python3-numpy
sudo apt install python-scipy  # scipy
sudo apt install python3-scipy
sudo apt install python-matplotlib  # matplotlib
sudo apt install python3-matplotlib
sudo apt install python-sklearn  # scikit-learn
sudo apt install python3-sklearn
sudo apt install python-skimage  # scikit-image
sudo apt install python3-skimage
sudo apt install python-opencv  # opencv
sudo apt install python3-opencv
sudo apt install python-pandas  # pandas
sudo apt install python3-pandas
sudo apt install python-pil  # pillow
sudo apt install python3-pil
sudo apt install python-pil.imagetk  # if the imageTk import doesn't work
sudo apt install python3-pil.imagetk
sudo apt install python-psutil  # psutil
sudo apt install python3-psutil
sudo apt install python-spur  # spur
sudo apt install python3-spur
sudo apt install cython  # cython
sudo apt install cython3
sudo apt install python-ipython  # ipython
sudo apt install python3-ipython
sudo apt install ipython
sudo apt install ipython3
sudo apt install jupyter  # jupyter
sudo apt install git  # git

# Mocking and testing library
sudo apt install python-mock
sudo apt install python3-mock

# pydot is a Python interface to Graphviz's dot
# pydot is needed for TensorFlow
sudo apt install graphviz
sudo apt install python-pydot
sudo apt install python3-pydot
sudo apt install python-pyparsing
sudo apt install python3-pyparsing

# To have both python 2.x and 3.x available on jupyter
sudo apt install python-ipykernel
sudo apt install python3-ipykernel
```

Install it ALL with one command :-)

However try to use [virtual environments](05_Virtual_environments.md) instead
to escape possible problems.

```shell
# Install it ALL with one command :-)
# However try to use virtual environments to escape possible problems.
sudo apt install python-pip         python2.7           \
                 python3-pip        python3             \
                 python-numpy       python3-numpy       \
                 python-scipy       python3-scipy       \
                 python-matplotlib  python3-matplotlib  \
                 python-sklearn     python3-sklearn     \
                 python-skimage     python3-skimage     \
                 python-opencv      python3-opencv      \
                 python-pandas      python3-pandas      \
                 python-pil         python3-pil         \
                 python-pil.imagetk python3-pil.imagetk \
                 python-psutil      python3-psutil      \
                 python-spur        python3-spur        \
                 cython             cython3             \
                 python-ipython     python3-ipython     \
                 ipython            ipython3            \
                 jupyter            git                 \
                 python-mock        python3-mock        \
                 graphviz                               \
                 python-pydot       python3-pydot       \
                 python-pyparsing   python3-pyparsing   \
                 python-ipykernel   python3-ipykernel   \
                 python-dev         python3-dev
```

Install TensorFlow for all users on the operating system,
but you should have [CUDA and cuDNN installed](08_Nvidia_driver_and_CUDA_install.md)
beforehand:

```shell
# Install CUDA beforehand
# Install cuDNN beforehand

# Change to root user
sudo su
# Change directory to HOME
cd ~
# Set permissions 644 for files and 755 for directories
umask 022
# Install TensorFlow current release with GPU support
# in the global path for Python 2.7
pip install tensorflow-gpu 
# For Python 3.x
pip3 install tensorflow-gpu

# Check it
python -c "import tensorflow as tf;  \
    tf.enable_eager_execution();     \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"
python3 -c "import tensorflow as tf;  \
    tf.enable_eager_execution();      \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"
```

---
### <a name="check" />Check installations

```shell
# Check installation for Python header files and a static library
sudo apt install python-dev
sudo apt install python3-dev
```

```shell
# Temporary set environment variable
export PATH=/usr/bin:$PATH
# To check python 2.x and 3.x run
python
# and
python3

# Then type in python 2.x or 3.x console
import numpy       # check NumPy
import scipy       # check SciPy
import matplotlib  # check MatPlotLib
import sklearn     # check Scikit-learn
import skimage     # check Scikit-image
import cv2         # check OpenCV
import pandas      # check Pandas
from PIL import ImageTk  # check Pillow
import psutil      # check PsUtil
import spur        # check Spur
import cython      # check Cython
exit()
```

```shell
# To check IPython run
ipython
exit
ipython3
exit
```

```shell
# To check Jupyter run
jupyter notebook
# and check both python versions 2.x and 3.x in "New" menu of the browser.
```

```shell
# To check GIT run
git --version
```

---
### <a name="additional" />Search and remove packages

```shell
# To remove package (don't remove python3 -- it'll broke your Ubuntu)
#sudo apt purge --auto-remove packagename
sudo apt remove packagename
# To search for the packagename
apt search packagename
```
