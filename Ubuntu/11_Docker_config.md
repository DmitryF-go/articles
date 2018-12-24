   - [Task](#task)
   - [Useful links](#useful)
   - [Close port 5000](#close-port)
   - [Stop all docker containers](#stop-all)

---
### <a name="task" />Task
   - Docker configuration.

---
### <a name="useful" />Useful links

   1. [How To Install and Use Docker on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)
   2. [Get Started, Part 1: Orientation and setup](https://docs.docker.com/get-started)
   3. []()
   4. []()

---
### <a name="stop-all" />Stop all docker containers

```shell
sudo docker kill $(sudo docker ps -q)
```

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
