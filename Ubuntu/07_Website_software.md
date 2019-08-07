   - [Task](#task)
   - [Allow user permissions](#permissions)
      - [Relative articles](#relative-articles)
      - [Allow to execute `sudo` commands](#allow-execute)
      - [Allow to write in the system folder](#allow-write)
   - [Packages for the website](#website)
   - [Work with Nginx server and its API](#work)
   - [Configure `nginx`](#configure)
   - [Correctly delete `nginx`](#nginx)

---
### <a name="task" />Task
   - Install software for the website http://image.org.by

---
### <a name="permissions" />Allow user permissions

:exclamation: **For trusted users only.** :exclamation:

#### <a name="relative-articles" />Relative articles
   * [Ubuntu Docs - Sudoers](https://help.ubuntu.com/community/Sudoers)
   * [How To Edit the Sudoers File on Ubuntu and CentOS](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos)
   * [Take Control of your Linux | sudoers file: How to with Examples](https://www.garron.me/en/linux/visudo-command-sudoers-file-sudo-default-editor.html)
   * [FilePermissionsACLs](https://help.ubuntu.com/community/FilePermissionsACLs)
   * [My answer on AskUbuntu.com](https://askubuntu.com/a/1098707/672237)

#### <a name="allow-execute" />Allow to execute `sudo` commands

Allow to execute `sudo` commands without granting root permissions.

Create and edit file with `visudo` editor
in the directory `/etc/sudoers.d/`.

```shell
# Create and edit file for 'website' group
sudo visudo -f /etc/sudoers.d/website

# Write into the file /etc/sudoers.d/website

# Create alias for WEBMASTERS group
User_Alias WEBMASTERS = username, vozman, romanroskach, malyshevvalery

# Create commands alias to start, stop and restart some services and view BIOS
Cmnd_Alias START1   = /bin/systemctl start nginx,          \
                      /bin/systemctl start generator,      \
                      /bin/systemctl start slide_analysis_api
Cmnd_Alias STOP1    = /bin/systemctl stop nginx,           \
                      /bin/systemctl stop generator,       \
                      /bin/systemctl stop slide_analysis_api
Cmnd_Alias RESTART1 = /bin/systemctl restart nginx,        \
                      /bin/systemctl restart generator,    \
                      /bin/systemctl restart slide_analysis_api
Cmnd_Alias STATUS1  = /bin/systemctl status nginx,         \
                      /bin/systemctl status generator,     \
                      /bin/systemctl status slide_analysis_api

Cmnd_Alias START2   = /usr/sbin/service nginx start,       \
                      /usr/sbin/service generator start,   \
                      /usr/sbin/service slide_analysis_api start
Cmnd_Alias STOP2    = /usr/sbin/service nginx stop,        \
                      /usr/sbin/service generator stop,    \
                      /usr/sbin/service slide_analysis_api stop
Cmnd_Alias RESTART2 = /usr/sbin/service nginx restart,     \
                      /usr/sbin/service generator restart, \
                      /usr/sbin/service slide_analysis_api restart
Cmnd_Alias STATUS2  = /usr/sbin/service nginx status,      \
                      /usr/sbin/service generator status,  \
                      /usr/sbin/service slide_analysis_api status

Cmnd_Alias FUSER1   = /bin/fuser 3000/tcp, /bin/fuser -k 3000/tcp
Cmnd_Alias FUSER2   = /bin/fuser 4000/tcp, /bin/fuser -k 4000/tcp
Cmnd_Alias FUSER3   = /bin/fuser 8080/tcp, /bin/fuser -k 8080/tcp
Cmnd_Alias FUSER4   = /bin/fuser 8081/tcp, /bin/fuser -k 8081/tcp
Cmnd_Alias FUSER5   = /bin/fuser 8083/tcp, /bin/fuser -k 8083/tcp

Cmnd_Alias STATUS   = /bin/systemctl status
Cmnd_Alias DAEMON   = /bin/systemctl daemon-reload
Cmnd_Alias BIOS     = /usr/sbin/dmidecode -t bios

# Allow members of WEBMASTERS to restart some services and view BIOS
WEBMASTERS ALL = START1, START2, STOP1, STOP2, RESTART1, RESTART2, STATUS1, STATUS2, \
                 FUSER1, FUSER2, FUSER3, FUSER4, FUSER5, STATUS, DAEMON, BIOS

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

sudo service nginx restart
sudo systemctl restart nginx
sudo service slide_analysis_api start

sudo fuser 3000/tcp  # view port 3000/tcp
```

#### <a name="allow-write" />Allow to write in the system folder

Prepare `webmasters` group:

```shell
# Check 'webmasters' group doen't exist
cat /etc/group | grep webmasters
# Create 'webmasters' group
sudo addgroup webmasters
# Add users to 'webmasters' group
sudo usermod -a -G webmasters username
sudo usermod -a -G webmasters vozman
sudo usermod -a -G webmasters romanroskach
sudo usermod -a -G webmasters malyshevvalery

# Group assignment changes won't take effect
# until the users log out and back in.
```

For `webmasters` group give write permission to directories:

```shell
/etc/systemd/system -- to start services automatically
/etc/nginx -- for Nginx
/etc/letsencrypt -- for Certbot

# List ACLs
getfacl /etc/nginx/
getfacl /etc/systemd/system
getfacl /etc/letsencrypt

    getfacl: Removing leading '/' from absolute path names
    # file: etc/systemd/system
    # owner: root
    # group: root
    user::rwx
    group::r-x
    other::r-x

# Add 'webmasters' group to an ACL
sudo setfacl -R -m g:webmasters:rwx /etc/nginx
sudo setfacl -R -m g:webmasters:rwx /etc/systemd/system
sudo setfacl -R -m g:webmasters:rwx /etc/letsencrypt

# Check
getfacl /etc/nginx
getfacl /etc/systemd/system
getfacl /etc/letsencrypt

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

Give read permission to files in the directory `/var/log/nginx`.

```shell
# There is read permission to the directory `/var/log/nginx` itself.
# But the owner of files in this directory is `www-data` and the group is `adm`.
ls -hal /var/log/nginx
    total 560K
    drwxr-xr-x  2 root     adm    4.0K Aug  7 00:12 .
    drwxrwxr-x 14 root     syslog 4.0K Aug  7 00:12 ..
    -rw-r-----  1 www-data adm    122K Aug  7 10:15 access.log
    -rw-r-----  1 www-data adm     26K Aug  5 23:58 access.log.2.gz
    -rw-r-----  1 www-data adm     12K Aug  7 10:09 error.log
    -rw-r-----  1 www-data adm     808 Aug  5 10:32 error.log.2.gz

# So add user to the `adm` group to read files in the directory `/var/log/nginx`.
# Add users to `adm` group.
cat /etc/group | grep adm
sudo usermod -a -G adm username
sudo usermod -a -G adm malyshevvalery
cat /etc/group | grep adm
```

---
### <a name="website" />Packages for the website

Install packages for http://image.org.by

Github repository with installation instructions https://github.com/Vozf/slide_analysis_api

```shell
sudo apt install git
sudo apt install python3
sudo apt install python3-pip
sudo apt install python3-tk
sudo apt install nodejs
sudo apt install npm
sudo apt install openslide-tools
sudo apt install libsm6
sudo apt install libxext6

# Python 2 and 3 wrappers for reading whole slide image files
sudo apt install python-openslide
sudo apt install python3-openslide

# Flask micro web framework
sudo apt install python-flask
sudo apt install python3-flask

# nginx [engine x] is an HTTP and reverse proxy server,
# a mail proxy server, and a generic TCP/UDP proxy server,
# originally written by Igor Sysoev.
sudo apt install nginx
```

---
### <a name="work" />Work with Nginx server and its API

- [Ubuntu Linux: Start / Restart / Stop Nginx Web Server](https://www.cyberciti.biz/faq/nginx-restart-ubuntu-linux-command)

```shell
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl status nginx
sudo systemctl status nginx.service
# or
sudo service nginx stop
sudo service nginx start
sudo service nginx restart
sudo service nginx status
# or
sudo nginx -s reload
# or
systemctl is-active nginx
systemctl is-active slide_analysis_api

# Check for the syntax error in config file
sudo nginx -t
# Check for nginx server log files
sudo tail -f /var/log/nginx/error.log
# or
sudo journalctl -xe

# There is API for Nginx server
sudo systemctl start slide_analysis_api
```

Two services: ```nginx``` and ```slide_analysis_api```, must start
automatically after computer reboots.
Service ```nginx``` start automatically after software installation
through SysV (```man update-rc.d```).
Service ```slide_analysis_api``` is using config file
```/etc/systemd/system/slide_analysis_api.service``` for autorun.
See [how-to run scripts on start up](02_How-tos.md/#autorun) for more details.

```shell
# Add slide_analysis_api to autorun
sudo systemctl daemon-reload
sudo systemctl enable slide_analysis_api.service
```

Check it: reboot and wait for 2-3 minutes for service to start.
You should see the images on the website.

---
### <a name="configure" />Configure `nginx`

Add configuration files to directories
`/etc/nginx/sites-enabled` and `/etc/nginx/sites-available`.

Open ports 80 for HTTP and 3000 for API

```shell
# Open ports. Allow incoming TCP and UDP packets.
sudo ufw allow 80
sudo ufw allow 3000
sudo ufw allow 8080
sudo ufw allow 8081
sudo ufw allow 8083

# Delete existing rule.
# Simply prefix the original rule with "delete".
## sudo ufw delete allow 80

# Check if it works

# Check the status of ufw
sudo ufw status
```

Ports: 80, 3000, 4000 are reserved for the website.
Website API works via 4000 port for localhost (not via IP).
With the help of `nginx` 3000 port redirected to 4000.

To check website works locally, enter in your browser's URL
field `http://localhost` or `http://127.0.0.1`.

To check website works globally, enter in your browser's URL
field `http://your-ip-address`. Website main page should open.

---
### <a name="nginx" />Correctly delete `nginx`

[Original answer here](https://askubuntu.com/questions/235347/what-is-the-best-way-to-uninstall-nginx)

```shell
# Removes all but config files
sudo apt remove nginx nginx-common
# Removes everything
sudo apt purge nginx nginx-common
# After using any of the above commands, use this
# in order to remove dependencies used by nginx
# which are no longer required
sudo apt autoremove
## rm -rf /etc/nginx  # to remove the conf files too
```
