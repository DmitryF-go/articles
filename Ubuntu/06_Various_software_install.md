   - [Task](#task)
   - [Installations](#install)
      - [All other different software](#other-software)
      - [Chromium](#Chromium)
      - [Elastix](#Elastix)
      - [Emacs](#Emacs)
      - [FileZilla](#filezilla)
      - [Geeqie](#Geeqie)
      - [JebBrains IntelliJ](#IntelliJ)
      - [Notepad++](#notepad-plus-plus)
      - [PyCharm Community](#PyCharm)
      - [Tmux](#Tmux)
      - [Ubuntu Software Center](#Ubuntu-Software-Center)
      - [uTorrent](#uTorrent)
   - [Starting any process in background mode](#background-mode)
   - [Snap commands](#snap)

---
### <a name="task" />Task

Install different software on Ubuntu 18.04.

---
### <a name="install" />Installations

----
#### <a name="other-software" />All other different software
```shell
sudo apt install htop      # CPU monitoring
sudo apt install git       # Git
sudo apt install mc        # Midnight Commander
sudo apt install autoconf  # automatic configure script builder
sudo apt install make      # utility for directing compilation
sudo apt install curl      # tool for transferring data with URL syntax
sudo apt install gcc gcc-6 g++ g++-6  # GCC and C++ compilers
sudo apt install net-tools   # ifconfig command
sudo apt install traceroute  # traceroute command 

# Or in one command
sudo apt install htop git mc autoconf make curl gcc gcc-6 g++ g++-6 \
                 net-tools traceroute
```

----
#### <a name="Chromium" />Chromium

```shell
# Install Chromium web browser
sudo apt install chromium-browser
# Start Chromium in terminal background
chromium-browser &
# If you don't want to see application output
chromium-browser &> /dev/null
```

----
#### <a name="Elastix" />Elastix

```shell
# Toolbox for rigid and nonrigid registration of images
sudo apt install elastix
# Documents for Elastix toolbox
sudo apt install elastix-doc
# Check installation
elastix --version
```

----
#### <a name="Emacs" />Emacs

```shell
# Emacs editor
sudo apt install emacs25
```

----
#### <a name="filezilla" />FileZilla

```shell
sudo apt-get install filezilla
filezilla&
```

----
#### <a name="Geeqie" />Geeqie

[Geeqie](https://www.linuxhelp.com/how-to-install-geeqie-in-ubuntu)
is an open source image viewer and organizer that allows the users to view graphics files.

It is useful to process images with it.

```shell
# Install image viewer using GTK+ and data files for Geeqie.
sudo apt install geeqie geeqie-common
geeqie &  # run Geeqie

# Uninstall Geeqie if you like
sudo apt remove geeqie
```

----
#### <a name="IntelliJ" />JebBrains IntelliJ

```shell
# Install JetBrains IntelliJ IDEA Community edition
# https://blog.jetbrains.com/idea/2017/11/install-intellij-idea-with-snaps
sudo snap install intellij-idea-community --classic

# Run JetBrains IntelliJ IDEA Community edition
intellij-idea-community &
```

----
#### <a name="notepad-plus-plus" />Notepad++

[Notepad++](https://www.tecrobust.com/install-notepad-plus-plus-linux-ubuntu)
is a Best Programming Text Editor and as well as a Source Code Editor which was developed
for Windows Platform and is more famous among the Windows users.
It provides a simplified speed typing interface with a lot of features such as Syntax Highlighting,
Brace Matching, AutoComplete, Color Codes, and a lot more features.
Source Code Editor also makes it very easy to run Compliers, debuggers, and more.
Though Notepad++ is not available for Linux, there is a way to use Notepadd++
on Linux Ubuntu Distributions.

```shell
sudo apt install snapd               # install Snap tool
sudo apt install wine-stable         # install Wine tool
sudo snap install notepad-plus-plus  # install Notepad++
notepad-plus-plus &                  # run Notepad++
```

----
#### <a name="PyCharm" />PyCharm Community

Install free of charge PyCharm Community edition.

[How-to install needed version](https://snapcraft.io/pycharm-community)

```shell
# Install PyCharm Community 2018.3 stable release
sudo snap install pycharm-community --channel=2018.3/stable --classic
# or the last stable version
sudo snap install pycharm-community --classic

pycharm-community &  # run it in background mode

# Remove possible error with Canberra Gtk.
# canberra-gtk-module translates GTK+ widgets signals to event sounds
# This module is needed for PyCharm successful start
sudo apt install libcanberra-gtk-module
```

----
#### <a name="Tmux" />Tmux

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

----
#### <a name="Ubuntu-Software-Center" />Ubuntu Software Center

[Ubuntu Software Center](https://www.ubuntupit.com/how-to-install-software-in-ubuntu-linux-a-complete-guide-for-newbie/)
is a GUI to install software for newbie.

```shell
sudo apt install ubuntu-software  # install Ubuntu Software Center
ubuntu-software &
```

----
#### <a name="uTorrent" />uTorrent

```shell
snap find torrent
snap install utorrent
utorrent &
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

To craft needed snap visit the [SnapCraft web-site](https://snapcraft.io/pycharm-community).

[How to Install and Use Snap on Ubuntu 18.04](https://codeburst.io/how-to-install-and-use-snap-on-ubuntu-18-04-9fcb6e3b34f9)

```shell
# Install Snap tool
sudo apt install snapd
snap --version
snap search pycharm  # search for PyCharm packages

# Use Snap tool
sudo snap find               # to list the available packages
sudo snap install <package>  # to install a package
sudo snap list               # to view all the installed snap packages
sudo snap changes            # to view a list of logged actions
sudo snap refresh <package>  # to upgrade a package to its latest available version
sudo snap refresh --list     # to see which packages have updates to be installed
sudo snap revert <package>   # to revert it to the previously installed version
sudo snap remove <package>   # to uninstall a package

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

# Temporary enable-disable package.
sudo snap disable <package>
sudo snap enable <package>

# Add /snap/bin/ directory to the $PATH
sudo nano /etc/environment
# Run environment file to effect changes.
. /etc/environment
```
