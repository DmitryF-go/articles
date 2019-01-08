   - [Task](#task)
   - [Useful links](#useful)
   - [Close port 5000](#close-port)
   - [Manage Docker as a non-root user](#non-root)
   - [Stop all docker containers](#stop-all)

---
### <a name="task" />Task
   - Docker configuration and administration.

---
### <a name="useful" />Useful links

   1. [How To Install and Use Docker on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)
   2. [Post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall)
   3. [Get Started, Part 1: Orientation and setup](https://docs.docker.com/get-started)
   4. []()

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
```

---
### <a name="stop-all" />Stop all docker containers

```shell
sudo docker kill $(sudo docker ps -q)
```
