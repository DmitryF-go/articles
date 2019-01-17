   - [Task](#task)
   - [Useful links](#useful)
   - [Close port 5000](#close-port)
   - [Manage Docker](#manage)
   - [Manage Docker as a non-root user](#non-root)
   - [Install DIGITS](#install)
   - [Start DIGITS](#start)
   - [Start DIGITS automatically](#autostart)
   - [Start containers automatically](#start-container)
   - [Stop all docker containers](#stop-all)

---
### <a name="task" />Task
   - Docker configuration and administration.
   - Nvidia DIGITS configuration and administration.

---
### <a name="useful" />Useful links

Docker links:
   - [How To Install and Use Docker on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)
   - [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall)
   - [Get Started, Part 1: Orientation and setup](https://docs.docker.com/get-started)
   - [Start containers automatically](https://docs.docker.com/config/containers/start-containers-automatically)

Nvidia DIGITS links:
   - [Getting Started With The DIGITS Container](https://docs.nvidia.com/deeplearning/digits/digits-container-getting-started/index.html)
   - [GitHub getting started](https://github.com/NVIDIA/DIGITS/blob/master/docs/GettingStarted.md)
   - []()

---
### <a name="close-port" />Close port 5000

Close port 5000 for security reasons.
You should login via SSH graphical interface
`ssh -X user@website -p port` or via X2Go Client
and then use `http://localhost:5000` to connect to DIGITS.

```shell
# Open port 5000 for NVIDIA DIGITS (not secure)
##sudo ufw allow 5000
# Close port 5000 for NVIDIA DIGITS
sudo ufw delete allow 5000
# Check the status
sudo ufw status
```

---
### <a name="manage" />Manage Docker

```shell
# To show only running containers
docker ps
# or
docker container ls

# To show all containers
docker ps -a
# or
docker container ls -a

# Check status of the docker service. Press "Q" key to exit.
sudo systemctl status docker
# or
systemctl is-active docker 
```

---
### <a name="non-root" />Manage Docker as a non-root user

If you don’t want to preface the ```docker``` command with ```sudo```,
create a Unix group called ```docker``` and add users to it.

:exclamation: **Only trusted users should be allowed
to control your Docker daemon.** :exclamation:

```shell
# Create the docker group.
sudo groupadd docker
# Add trusted users to the docker group.
# I don't know, is it possible to add existing group to docker group?
sudo usermod -aG docker $USER
# Log out and log back.

# Verify that you can run docker commands without sudo.
docker run hello-world

# To delete user from a docker group
sudo deluser username docker
```

---
### <a name="install" />Install DIGITS

Before running the application, use the ```docker pull``` command
to ensure an up-to-date image is installed.

```shell
sudo docker pull nvidia/digits:latest
# or
sudo docker pull nvcr.io/nvidia/digits:18.04
```

---
### <a name="start" />Start DIGITS

```shell
# Run DIGITS with Nvidia Docker
sudo nvidia-docker run -v /hdd_purple:/data/ -p 5000:5000 nvidia/digits:latest
```

---
### <a name="autostart" />Start DIGITS automatically

Warning: It is not tested yet!

Create ```/etc/systemd/system/mydigits.service``` containing:

```shell
[Unit]
Description=Autostart NVIDIA DIGITS
Requires=docker.service
After=docker.service

[Service]
User=foobar167
WorkingDirectory=/usr/bin/
ExecStart=/usr/bin/nvidia-docker run -v /hdd_purple:/data/ -p 5000:5000 nvidia/digits:latest

[Install]
WantedBy=multi-user.target
```

And then run:

```shell
# Add slide_analysis_api to autorun
sudo systemctl daemon-reload
sudo systemctl enable mydigits.service
```

---
### <a name="start-container" />Start containers automatically

It starts container, but not DIGITS itself.

```shell
# Autostart
docker run -dit --restart unless-stopped nvidia/digits:latest

# Stop autostart
docker run -dit --restart no nvidia/digits:latest
```

---
### <a name="stop-all" />Stop all docker containers

```shell
sudo docker kill $(sudo docker ps -q)
```
