   - [Task](#task)
   - [Useful links](#links)
   - [Packages for the website](#website)
   - [Work with Nginx server](#work)
   - [Allow user to execute root commands](#grant)
   - [Correctly delete `nginx`](#nginx)

---
### <a name="task" />Task
   - Install software for the website http://image.org.by

---
### <a name="links" />Useful links
   - [Ubuntu Linux: Start / Restart / Stop Nginx Web Server](https://www.cyberciti.biz/faq/nginx-restart-ubuntu-linux-command)

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
### <a name="work" />Work with Nginx server and API for it

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

---
### <a name="grant" />Allow user to execute root commands

[See how-to execute root commands and write permission into folders](02_How-tos.md#exec)

For *webmasters* group allow to execute commands:
```shell
sudo systemctl restart nginx
sudo systemctl start slide_analysis_api
```

For *webmasters* group allow to write into folders:

```shell
/etc/systemd/system/
/etc/nginx/

# Set access permissions to write into folder
sudo setfacl -m g:webmasters:rwx /etc/nginx

# View access permisisons
getfacl /etc/nginx

getfacl: Removing leading '/' from absolute path names
# file: etc/nginx
# owner: root
# group: root
user::rwx
group::r-x
group:webmasters:rwx
mask::rwx
other::r-x
```

---
### <a name="configure" />Configure `nginx`

Add configuration files to directories
`/etc/nginx/sites-enabled` and `/etc/nginx/sites-available`.

Open ports 80 for HTTP and 3000 for API

```shell
# Open ports 80 for HTTP and 3000 for API.
# Allow incoming TCP and UDP packets.
sudo ufw allow 80
sudo ufw allow 3000

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
