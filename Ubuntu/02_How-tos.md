How-to:
   - [Add directory to the $PATH](#add_to_path)
   - [Allow user to execute root commands](#exec)
   - [Calculator](#calculator)
   - [Clear out Git history](#clear-git)
   - [Create user](#user)
   - [Change password](#passwd)
   - [Check Linux version](#check-linux)
   - [Delete package](#delete)
   - [Find file by name](#find)
   - [Install deb file](#deb)
   - [Kill the tty](#tty-kill)
   - [List all environment variables](#printenv)
   - [Lock Screen](#lock)
   - [Manage software updates](#software-updates)
   - [Mount USB](#mount)
   - [Open console](#console)
   - [Run scripts on start up](#autorun)
   - [Set environment variable](#envvar)
   - [Show the model of the computer](#show_the_model)
   - [Switch language hotkey](#lang)
   - [Take screenshot](#screenshot)
   - [View computer resources](#resources)
   - [View disk usage](#disk-usage)
   - [View screen resolution](#resolution)
   - [Who is logged in](#who)

---
### <a name="add_to_path" />Add directory to the $PATH

```shell
# Temporary addition
export PATH=$PATH:/usr/bin

# Permanent for ALL users on the operating system
sudo nano /etc/environment
# Edit the file. Make sure that the path is something like this:
# PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
# Run environment file to effect changes.
. /etc/environment

# For the local current user
sudo nano ~/.bashrc
# Add the following line to the bashrc file:
# export PATH="/path/to/dir:$PATH"
# Run bashrc file to effect changes.
source ~/.bashrc
```

---
### <a name="exec" />Allow user to execute root commands

:exclamation: **For trusted users only.** :exclamation:

Relative articles:
   * [Ubuntu Docs - Sudoers](https://help.ubuntu.com/community/Sudoers)
   * [How To Edit the Sudoers File on Ubuntu and CentOS](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos)
   * [Take Control of your Linux | sudoers file: How to with Examples](https://www.garron.me/en/linux/visudo-command-sudoers-file-sudo-default-editor.html)
   * [FilePermissionsACLs](https://help.ubuntu.com/community/FilePermissionsACLs)
   * [My answer on AskUbuntu.com](https://askubuntu.com/a/1098707/672237)

*Allow to execute certain commands
without granting root permissions*

Create and edit file with `visudo` editor
in the directory `/etc/sudoers.d/`.

```shell
# Create and edit file for 'website' group
sudo visudo -f /etc/sudoers.d/website

# Write into the file /etc/sudoers.d/website

# Create alias for WEBMASTERS group
User_Alias WEBMASTERS = username, vozman, romanroskach

# Create commands alias to start, stop and restart some services and view BIOS
Cmnd_Alias START1   = /bin/systemctl start nginx,    /bin/systemctl start slide_analysis_api
Cmnd_Alias STOP1    = /bin/systemctl stop nginx,     /bin/systemctl stop slide_analysis_api
Cmnd_Alias RESTART1 = /bin/systemctl restart nginx,  /bin/systemctl restart slide_analysis_api

Cmnd_Alias START2   = /usr/sbin/service nginx start,   /usr/sbin/service slide_analysis_api start
Cmnd_Alias STOP2    = /usr/sbin/service nginx stop,    /usr/sbin/service slide_analysis_api stop
Cmnd_Alias RESTART2 = /usr/sbin/service nginx restart, /usr/sbin/service slide_analysis_api restart

Cmnd_Alias BIOS     = /usr/sbin/dmidecode -t bios

# Allow members of WEBMASTERS to restart some services and view BIOS
WEBMASTERS ALL = START1, STOP1, RESTART1, START2, STOP2, RESTART2, BIOS

```

Check it or edit broken configuration file:
```shell
# To edit broken configuration file
pkexec visudo -f /etc/sudoers.d/website

# Check if it works -- view BIOS as 'username' (for root)
sudo -u username sudo dmidecode -t bios    # should work
sudo -u username sudo dmidecode -t memory  # should NOT work

# Check under "username" account
sudo dmidecode -t bios    # should work
sudo dmidecode -t memory  # should NOT work
sudo /usr/sbin/service nginx restart
sudo /bin/systemctl restart nginx
sudo /usr/sbin/service slide_analysis_api start
```

*Allow to write in the system folder*

Give write permission to `/etc/nginx/` folder.

```shell
# Check 'webmasters' group doen't exist
cat /etc/group | grep webmasters
# Create 'webmasters' group
sudo addgroup webmasters
# Add users to 'webmasters' group
sudo usermod -a -G webmasters username
sudo usermod -a -G webmasters vozman
sudo usermod -a -G webmasters romanroskach

# Group assignment changes won't take effect
# until the users log out and back in.

# Create directory
sudo mkdir /etc/nginx/
# Check directory permissions
ls -al /etc | grep nginx
drwxr-xr-x   2 root root     4096 Dec  5 18:30 nginx

# Change group owner of the directory
sudo chgrp -R webmasters /etc/nginx/
# Check that the group owner is changed
ls -al /etc | grep nginx
drwxr-xr-x   2 root webmasters   4096 Dec  5 18:30 nginx

# Give write permission to the group
sudo chmod -R g+w /etc/nginx/
# Check
ls -al /etc | grep nginx
drwxrwxr-x   2 root webmasters   4096 Dec  5 18:30 nginx

# Try to create file
sudo -u username touch /etc/nginx/test.txt  # should work
sudo -u username touch /etc/test.txt  # Permission denied
```

Give write permission to `/etc/systemd/system/` folder.

```shell
# List ACLs
getfacl /etc/systemd/system

getfacl: Removing leading '/' from absolute path names
# file: etc/systemd/system
# owner: root
# group: root
user::rwx
group::r-x
other::r-x

# Add 'webmasters' group to an ACL
sudo setfacl -m g:webmasters:rwx /etc/systemd/system

# Check
getfacl /etc/systemd/system

getfacl: Removing leading '/' from absolute path names
# file: etc/systemd/system
# owner: root
# group: root
user::rwx
group::r-x
group:webmasters:rwx
mask::rwx
other::r-x

sudo -u username touch /etc/systemd/system/test.txt  # should work
sudo -u username touch /etc/systemd/test.txt  # Permission denied
```

---
### <a name="calculator" />Calculator

```shell
gnome-calculator &> /dev/null
```

---
### <a name="clear-git" />Clear out Git history

Steps to [clear out the history](https://gist.github.com/stephenhardy/5470814)
of a git/github repository.

```shell
# Remove history from Git directory
rm -rf .git

# Recreate the repos from the current content only
git init
git add .
git commit -m "Initial commit"

# Push to the github remote repos ensuring you overwrite history
git remote add origin https://github.com/foobar167/articles
# Or try this: git remote add origin git@github.com:<YOUR ACCOUNT>/<YOUR REPOS>.git
# Or try this: git remote add origin https://github.com/USERNAME/REPOSITORY.git
# But try it from the beginning (rm -rf .git, etc.)
git push -u --force origin master
```

---
### <a name="user" />Create user

   * Create user with GUI

Press `<Super>` (`<Win>`) key and type *Users*.
Users and groups window with *Users Settings* will open

![Users Settings window](data/2018.12.05_users_settings.png)

Click *Add* button to add new user.

   * Create user via console

Press `<Ctrl>+<Alt>+<T>` and open console window.

```shell
# Show list of all users
cat /etc/passwd
# or
cut -d: -f1 /etc/passwd

# Show list of all goups
cat /etc/group

# To create user with questions, directory and sceleton files
sudo adduser username
# Create and add user to the group
sudo adduser username groupname
# or without questions, dir and files (not recommended)
#sudo useradd username

# To delete user
sudo deluser username
# or (not recommended)
#sudo userdel username
# Delete directory
#sudo rm -r /home/username  # use with caution!

# To delete user from a group
sudo deluser username groupname

# Change user name
usermod -l new_username old_username

# Change user password
sudo passwd username

# Change shell for a user
sudo chsh username

# Change user detailes (name, phone, etc.)
sudo chfn username

# Create group
sudo addgroup groupname
# Add existing user to the group
sudo usermod -a -G groupname username
```

Enable or disable the user account:
```shell
sudo adduser tempuser       # create temporal user
sudo usermod -L tempuser    # disable user account
sudo usermod -U tempuser    # enable user account
sudo deluser tempuser       # delete user
sudo rm -r /home/tempuser   # use with caution!
cat /etc/group | grep temp  # check
ls /home | grep temp        # check
# If error: "userdel: user tempuser is currently used by process 1864"
sudo kill -9 1864           # kill the process
sudo deluser -f tempuser    # try again
```

---
### <a name="passwd" />Change password

*Change Password from GUI*

Click the system menu at the top right corner,
select your user name and click on *Account Settings* menu.

![Account Settings menu](data/2018.12.03_account_settings.png)

*Users* window will open.

![Account Settings window](data/2018.12.03_users.png)

On the *Users* window select *Password*. 

![Account Settings window](data/2018.12.03_change_passwd.png)

Enter current password. Enter new password, verify new password once again
and click on *Change* button.

*Change Password from Command Line*

[Open console](#console) and enter `passwd`.
Type current password, type new password and retype new password. 

```shell
passwd
Changing password for lab225.
(current) UNIX password: 
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
lab225@deeplab3:~$

# To change other users password
sudo passwd username
```

---
### <a name="check-linux" />Check Linux version

[How to check os version in Linux command line](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line)

```shell
# Find OS name and version
cat /etc/os-release
lsb_release -a
hostnamectl

# Find kernel version
uname -r
# Find Linux distribution
lsb_release -cs

# Even more options
cat /proc/version
cat /etc/issue
```

---
### <a name="delete" />Delete package

```shell
# Remove package
sudo apt remove packagename
# Remove user data and configuration files
sudo apt purge packagename

# Remove unused dependences
sudo apt autoremove
# Clean
sudo apt clean

# Or do this
sudo apt purge --auto-remove packagename
```

---
### <a name="find" />Find file by name

[How to search files from the terminal on Linux](https://www.howtoforge.com/tutorial/linux-search-files-from-the-terminal)
[14 Practical Examples of Linux Find Command for Beginners](https://www.howtoforge.com/tutorial/linux-find-command)

`locate` command

The reason for this unmatched speed is that the `locate` command isn't actually searching your local hard disks
for the files or directories that you need to find, but more like reads through the `mlocate.db`
database file which contains all file paths in your system.

```shell
sudo apt install locate  # install locate tool
sudo updatedb  # update the mlocate.db database before first use
locate $HOME/*.py  # search for Python files in the $HOME dir
locate -c $HOME/*.py  # count Python files in the $HOME dir
locate -ic $HOME/*.py  # count Python files and ignore case

# Example
sudo updatedb
locate -ic Home
1491350
locate -c Home
3
locate Home
/etc/gufw/Home.profile
/usr/share/man/man3/FcConfigEnableHome.3.gz
/usr/share/man/man3/FcConfigHome.3.gz
```

`find` command

The “find” command is a much more powerful but also slower searching utility.
Contrary to the `locate` command, `find` actually searching your disks for the files
and directories that the user is after.
`find` can search for files that belong to a certain user or group of users,
files that were modified or accessed recently, files that of a specific size range,
hidden files, executables, read-only files, and files with certain permissions.

```shell
find / -name "*name*" 2> /dev/null  # search in / root directory (everything)
find / -iname "*name*" 2> /dev/null  # ignore case
# Search in $HOME dir for Python files bigger than 1MB of size.
find ~/ -size +1M -iname "*.py" 2> /dev/null
# Search for Python files bigger than 700 kB of size.
find / -iname "*.py" -and -size +700k 2> /dev/null

# This does not work and I donot why: find / -iname "*.py" -and -size +700k -and -size -1M 2> /dev/null
# BUT this works for Ubuntu 18.04 :-) Note: 1M vs 1000k
find / -iname '*.py' -and -size +700k -and -size -1000k 2> /dev/null

find ~/ -cmin  -3 2> /dev/null  # changed  less than 3 minutes ago
find ~/ -ctime -3 -iname "*.png" 2> /dev/null  # changed less than 3 days ago
find ~/ -amin  -3 2> /dev/null  # accessed less than 3 minutes ago
find ~/ -atime -3 -iname "*.png" 2> /dev/null  # accessed less than 3 days ago

find . -iname "*.jpg" | wc -l    2>/dev/null  # count JPG files in the local dir
```

---
### <a name="deb" />Install deb file

```shell
sudo dpkg -i my.deb
```

---
### <a name="tty-kill" />Kill the tty

```shell
# Show who is logged in
who
username :1           2018-12-22 17:11 (:1)
username pts/7        2018-12-24 10:54 (178.120.33.235)
username pts/9        2018-12-24 12:17 (178.120.33.235)

# Get the PIDs
ps -ft pts/7
UID        PID  PPID  C STIME TTY          TIME CMD
username 25058 25057  0 10:54 pts/7    00:00:00 -bash
root     26850 25058  0 11:09 pts/7    00:00:00 su admin
admin    26859 26850  0 11:09 pts/7    00:00:00 bash
admin    26878 26859  0 11:09 pts/7    00:00:00 mc

# Use the PIDs to kill the processes
kill 25058 26850 26859 26878
-bash: kill: (26859) - Operation not permitted
-bash: kill: (26878) - Operation not permitted

# If the process doesn't gracefully terminate,
# forcefully kill it by sending a SIGKILL
kill -9 26859 26878
# or
kill -SIGKILL 26859 26878
```

---
### <a name="printenv" />List all environment variables

```shell
printenv  # list all or part of environment
env  # list all or run a program in a modified environment
# or
printenv | less
# or
printenv | more
```

[A list of the commonly used variables in Linux](https://www.cyberciti.biz/faq/linux-list-all-environment-variables-env-command)

---
### <a name="lock" />Lock Screen

To lock your screen press keys `<Ctrl>+<Alt>+<L>`.
If it does n't work then press keys `<Win>+<L>` or
(`<Super>+<L>`).

Also click on the *System* icon in the top right corner
of the screen and select *Lock Screen* icon/menu.

From console type:
```shell
# Lock the screen
gnome-screensaver-command -l
```

When your screen is locked, and you want to unlock it,
press `<Esc>`, or swipe up from the bottom of the screen
with your mouse. Then enter your password, and press
`<Enter>` or click *Unlock*.
Alternatively, just start typing your password and the lock
curtain will be automatically raised as you type.

---
### <a name="software-updates" />Manage software updates

- [How can PPAs be removed?](https://askubuntu.com/questions/307/how-can-ppas-be-removed)
- [How can I fix a 404 Error when using a PPA or updating my package lists?](https://askubuntu.com/questions/65911/how-can-i-fix-a-404-error-when-using-a-ppa-or-updating-my-package-lists)

Launch Menu --> Preferences --> Software & Updates

![Software & Updates](data/2019.03.13_software_updates.png)

Use sheet "Other Software" to manage repositories for updates.
Use sheet "Updates" to manage updates check.

Also check file `/etc/apt/sources.list` and directory `/etc/apt/sources.list.d`
which containes list of sources to update Ubuntu 18.04.

---
### <a name="mount" />Mount USB

By default, storage devices that are plugged into the system
mount automatically in the `/media/<username>` directory.

---
### <a name="console" />Open console

Press `<CTRL>+<ALT>+<T>` keys or press `<Win>` key and enter `terminal`.

---
### <a name="autorun" />Run scripts on start up

There are several ways to run scripts on start up:
   1. systemd (```man systemd.service```)
   2. SysV (```man update-rc.d```). System V init (also known as classic init).
   3. Add script to ```/etc/rc.local``` file.
   4. cron (```man cron```)
   5. Use ```~/.config/systemd/``` or ```/.config/autostart/``` directories
      for user sessions. Only after user login.

For Ubuntu 14.04 **and older** one can use [Upstart](http://upstart.ubuntu.com/getting-started.html).

1\. Create ```systemd``` unit files. Links:
  - [How to run scripts on start up](https://askubuntu.com/a/719157/672237)
  - [Writing unit files](https://wiki.archlinux.org/index.php/systemd#Writing_unit_files)
  - [systemd.service - Service unit configuration](http://manpages.ubuntu.com/manpages/xenial/en/man5/systemd.service.5.html)
  - [systemd.service - Service unit configuration. Copy](https://www.freedesktop.org/software/systemd/man/systemd.service.html)

Create ```/etc/systemd/system/foo.service``` containing:

```shell
[Unit]
Description=uWSGI instance to serve slide_analysis_api
After=network.target

[Service]
User=vozman
Group=www-data
WorkingDirectory=/home/vozman/slide_analysis/slide_analysis_api
Environment="PATH=/home/vozman/slide_analysis/slide_analysis_api"
ExecStart=/home/vozman/slide_analysis/slide_analysis_api/venv/bin/uwsgi --ini /home/vozman/slide_analysis/slide_analysis_api/slide_analysis_api.ini

[Install]
WantedBy=multi-user.target
```

Then run:

```shell
# Add slide_analysis_api to autorun
sudo systemctl daemon-reload
sudo systemctl enable slide_analysis_api.service
```
You can run multiple commands from the same service file,
using multiple ```ExecStart``` lines:

```shell
[Service]
ExecStart=/some/command
ExecStart=/another/command some args
ExecStart=-/a/third/command ignore failure
```

The command must always be given with the full path.
If any command fails, the rest aren't run.
A ```-``` before the path tells systemd to ignore a non-zero exit status
(instead of considering it a failure).

2\. Create script file ```/etc/init.d/myscript.sh```. Run the commands:

```shell
# To add use
sudo update-rc.d myscript.sh defaults  # default run levels are: 2,3,4 and 5
# To remove use
sudo update-rc.d -f myscript.sh remove
```

More info:
   - [Part 1: How to config service to autostart after crash or reboot](https://www.digitalocean.com/community/tutorials/how-to-configure-a-linux-service-to-start-automatically-after-a-crash-or-reboot-part-1-practical-examples)
   - [Part 2: How to config service to autostart after crash or reboot](https://www.digitalocean.com/community/tutorials/how-to-configure-a-linux-service-to-start-automatically-after-a-crash-or-reboot-part-2-reference)
   - [How to Enable or Disable Services in Ubuntu Systemd/Upstart](https://linoxide.com/linux-how-to/enable-disable-services-ubuntu-systemd-upstart)
   - [How can I configure a service to run at startup](https://askubuntu.com/a/9384/672237)

3\. Edit the file ```/etc/rc.local```.
```shell
sudo nano /etc/rc.local
```

Add commands, but make sure that the line ```exit 0``` is at the end.

If command runs continuously (in an infinite loop) or is likely not to exit,
you must be sure to fork the process by adding an ampersand ```&``` to the end
of the command, like so:

```shell
python3 /home/pi/myscript.py &
```

:exclamation: **Otherwise, the script will not end
and the system will not boot.** :exclamation:

The ampersand allows the command to run in a separate process and continue
booting with the process running.

4\. One approach is to add an @reboot [cron](https://en.wikipedia.org/wiki/Cron) task:
   1. Running ```crontab -e``` will allow you to edit your cron.
   2. Adding a line like this to it: ```@reboot /path/to/script```
      will execute that script once your computer boots up.

5\. For user sessions, you can create the systemd unit in
```~/.config/systemd``` instead. This should work with 16.04 onwards,
but not earlier releases of Ubuntu with systemd
(since those still used Upstart for user sessions).
User session units can be controlled with the same commands
as with system services, but with the ```--user``` option added:

```shell
systemctl --user daemon-reload
systemctl --user status foo.service
```

A shell script named ```.gnomerc``` in your home directory is automatically
sourced each time you log in to a GNOME session. You can put arbitrary
commands in there; environment variables that you set in this script
will be seen by any program that you run in your session.

Note that the session does not start until the ```.gnomerc``` script
is finished; therefore, if you want to autostart some long-running program,
you need to append ```&``` to the program invocation,
in order to detach it from the running shell.

The menu option **System -> Preferences -> Startup Applications** allows 
to define what applications should be started when your graphical session starts
(Ubuntu predefines quite some). This has almost the same purpose and scope
of the ```.gnomerc``` script, except you don't need to know ```sh``` syntax
(but neither can you use any ```sh``` programming construct).

---
### <a name="envvar" />Set environment variable

```shell
PYTHONPATH=/usr/lib/python3/dist-packages/caffe
export PYTHONPATH
```

---
### <a name="#show_the_model" />Show the model of the computer

```shell
# The first line will be product name.
sudo lshw | grep product
# or
sudo dmidecode -t system

sudo dmidecode -s bios-release-date       # show release date
sudo dmidecode -s baseboard-product-name  # show product name
sudo dmidecode -s baseboard-manufacturer  # show manufacturer

# Install GUI application
sudo apt install sysinfo
sysinfo&
```

---
### <a name="lang" />Switch language hotkey

On Ubuntu 18.04 the default shortcut is `<Win>+<Space>`.
`<Win>` key is also called `<Super>` key.

[Instructions](https://askubuntu.com/a/1029605/672237)
for Ubuntu 18.04 LTS with Gnome desktop from Gnome Tweaks.

```shell
# Install Gnome Tweaks
sudo apt-get install gnome-tweaks
# Open Gnome Tweaks
sudo gnome-tweaks &
```
   - Open *Gnome Tweaks* (`gnome-tweaks &`).
   - Select *Keyboard & Mouse* tab
   - Click *Additional Layout Options* button
   - Expand *Switching to another layout* menu
   - Select checkbox *Alt+Shift* there

![Gnome Tweaks Keyboard & Mouse](data/2018.12.03_keyboard_layout.png)

---
### <a name="screenshot" />Take screenshot

[How to take a screenshot on Ubuntu 18.04](https://linuxconfig.org/how-to-take-a-screenshot-on-ubuntu-18-04-bionic-beaver-linux)

Press `<Win>` key on the keyboard and enter `screenshot`.
Or enter in console `gnome-screenshot&`, but this works only in Gnome.
Screen shot application will appear.

   * Flameshot GUI
```shell
sudo apt install flameshot  # install Flameshot
flameshot gui  # start it
```
There is an icon on the panel menu after `flameshot gui` is started.

   * Kazam (Flameshot tool is better)
```shell
sudo apt install kazam  # install Kazam
kazam&  # start it in background mode
```

---
### <a name="resources" />View computer resources

   * GPU

```shell
# View NVIDIA resources
nvidia-smi
# or in non-stop mode (exit with Ctrl+C)
watch -n 0.5 nvidia-smi
```

   * CPU

```shell
# Check memory and CPU usage per process
top
```

   * HTOP

HTOP is a lightweight text-mode process viewer packed with handy features
such as killing processes without entering their PID,
displaying full command lines, etc with a colour display.

```shell
# Install HTOP interactive processes viewer
sudo apt install htop
# Run it
htop
# To quit press F10 or <Q> key.
```
![`htop` interactive viewer](data/2018.12.04_htop_tool.png)

   * System monitor GUI
   
Press `<Super>` or `<Win>` key, type "system monitor".

Run *Gnome System Monitory* from command line:

```shell
# Run Gnome System Monitor in background
gnome-system-monitor &
```

![System monitor GUI](data/2018.12.04_system_monitor.png)

   * Show system status

```shell
systemctl status
```

Navigate with arrow keys and
`<Home>`, `<End>`, `<Page Up>`, `<Page Down>` keys.
Exit with `<Q>` key.

![`systemctl status` command](data/2018.12.04_systemctl_status.png)

   * Memory usage

```shell
# Display memory usage in MBs
free -m
# Read /proc/meminfo file
cat /proc/meminfo
# Memory usage statistics
vmstat -s
```

   * Hardware info

```shell
# DMI table decoder
sudo dmidecode
# Show BIOS info
sudo dmidecode -t bios
```

---
### <a name="disk-usage" />View disk usage

```shell
gnome-disks&  # open disks utility in background mode

# Disk Usage Analyzer - view only bootable disk
sudo apt install baobab  # install it
baobab&  # launch it

df -h /home/
sudo du -sh /home 2> /dev/null
```

---
### <a name="resolution" />View screen resolution

`xrandr`

or open *System Settings* → *Display*.
There you'll see a *Resolution* drop-down menu.

---
### <a name="who" />Who is logged in

```shell
who
```
