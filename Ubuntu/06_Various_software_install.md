   - [Task](#task)
   - [Installations](#install)
   - [Anaconda](#anaconda)
   - [Starting any process in background mode](#background-mode)
   - [Snap commands](#snap)

---
### <a name="task" />Task

Install different software on Ubuntu 18.04.

---
### <a name="install" />Installations

**Chromium**

```shell
# Install Chromium web browser
sudo apt install chromium-browser
# Start Chromium in terminal background
chromium-browser &
# If you don't want to see application output
chromium-browser &> /dev/null
```

**Elastix**

```shell
# Toolbox for rigid and nonrigid registration of images
sudo apt install elastix
# Documents for Elastix toolbox
sudo apt install elastix-doc
# Check installation
elastix --version
```

**Emacs**

```shell
# Emacs editor
sudo apt install emacs25
```

**JebBrains IntelliJ**

```shell
# Install JetBrains IntelliJ IDEA Community edition
# https://blog.jetbrains.com/idea/2017/11/install-intellij-idea-with-snaps
sudo snap install intellij-idea-community --classic --edge

# Run JetBrains IntelliJ IDEA Community edition
intellij-idea-community &
```

**Tmux**

```shell
# Tmux terminal multiplexer
sudo apt install tmux

# Tmux session manager
sudo apt install tmuxp

# Tmux plugin manager based on git
sudo apt install tmux-plugin-manager

# Create and manage tmux sessions easily
sudo apt install tmuxinator

# Python 2.x and 3.x scripting library and ORM for tmux
sudo apt install python-libtmux
sudo apt install python3-libtmux

# Tmux session manager (Python 2.x and 3.x)
sudo apt install python-tmuxp
sudo apt install python3-tmuxp

# Terminal multiplexer with instant terminal sharing
sudo apt install tmate
```

[Using Tmux](https://hackernoon.com/a-gentle-introduction-to-tmux-8d784c404340)

![Tmux multiple panes](data/2019.02.13-tmux-multiple-panes.png)

```shell
# Start a session
tmux new -s nvidia-digits
# Attach a session
tmux attach-session -t [name of session]
# Check active sessions
tmux ls

# To split a pane horizontally press <ctrl>+<b> and then press <">:
ctrl+b "
# To split pane vertically:
ctrl+b %
# To move from pane to pane press <ctrl>+<b> and then press arrow key:
ctrl+b [arrow key]
# Cycle through panes:
ctrl+b o
# Resize pane:
ctrl+b :
resize-pane -D 2
# Kill current pane:
ctrl+b x

# Detach from session:
ctrl+b d
# Kill named session:
tmux kill-session -t [name of session]
# Kill tmux server, along with all sessions:
tmux kill-server
```

**uTorrent**

```shell
snap find torrent
snap install utorrent
utorrent
```

---
### <a name="anaconda" />Anaconda

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
# Activate the new environment
source activate myenv

# After activation of virtual environment,
# install TensorFlow using Anaconda
conda install tensorflow-gpu
# Check it
python -c "import tensorflow as tf;  \
    tf.enable_eager_execution();      \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"

# Activate and deactivate virtual environment
source deactivate
source activate myenv

# Install necessary modules into virtual environment
conda install opencv
conda install joblib
conda install scikit-image
conda install scikit-learn
conda install pandas
conda install caffe
conda install tqdm
conda install torchvision
conda install keras

# Downgrade TensorFlow to version 1.8.0
conda install tensorflow-gpu==1.8.0

# Start the test script
cd /hdd_purple/paulenka_XRSeg_temp/
python xrseg.py test_imgs/test0.png
```

---
### <a name="background-mode" />Starting any process in background mode

```shell
tmux a -t nvidia-digits  # attach an existing session
# Start any process in background mode
ctrl+b d  # detach from session
# Exit from SSH or log out from the system, but process will run.
```

---
### <a name="snap" />Snap commands

`snap` tool to interact with *snaps*.
*Snaps* are packages that are mainly designed to be sandboxed and isolated
from other system software, secure, and easily installable, upgradeable,
degradable, and removable irrespective of its underlying system.

[Video turorials](https://utappia.org/2016/04/22/how-to-search-install-remove-snap-packages-in-ubuntu)

```shell
sudo snap find                    # to list the available packages
sudo snap install <package name>  # to install a package
sudo snap list                    # to view all the installed snap packages
sudo snap changes                 # to view a list of logged actions
sudo snap refresh <package name>  # to upgrade a package to its latest available version
sudo snap remove <package name>   # to uninstall a package

# To update all snap packages
sudo snap refresh

# To remove broken snap installation
snap changes
# The output is:
#   ID   Status  Spawn               Ready  Summary
#   11   Doing   today at 16:15 +03  -      Install "intellij-idea-community" snap
sudo snap abort 11
# or
sudo snap remove intellij-idea-community
```
