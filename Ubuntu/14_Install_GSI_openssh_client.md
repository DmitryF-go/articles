   - [Task](#task)
   - [Links to read](#links)
   - [Install GSI openssh client](#install)
   - [Login via GSI SSH client](#configure)

---
### <a name="task" />Task
   - Install and configure GSI SSH client for **Ubuntu 18.04**.

---
### <a name="links" />Links to read
   * [Install GSI openssh client (Ubuntu and Debian)](https://www.lsc-group.phys.uwm.edu/lscdatagrid/doc/installgsiopensshclient.html)

---
### <a name="install" />Install GSI openssh client

GSI SSH client installation:
```shell
# Add repository to the /etc/apt/sources.list file
sudo nano /etc/apt/sources.list
# Copy-paste this line
deb http://www.globus.org/ftppub/gt5/5.2/stable/packages/deb/ubuntu/14.04 trusty contrib
# Update the repository definitions
sudo apt update
# Install the gsi-openssh-clients package
sudo apt install gsi-openssh-clients

# This should install gsissh, gsiscp and gsisftp.
gsissh
usage: ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]
           [-D [bind_address:]port] [-E log_file] [-e escape_char]
           [-F configfile] [-I pkcs11] [-i identity_file]
           [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec]
           [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address]
           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
           [user@]hostname [command]
gsiscp
usage: gsiscp [-12346BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file]
           [-l limit] [-o ssh_option] [-P port] [-S program]
           [[user@]host1:]file1 ... [[user@]host2:]file2
gsisftp
usage: gsisftp [-1246aCfpqrv] [-B buffer_size] [-b batchfile] [-c cipher]
          [-D sftp_server_path] [-F ssh_config] [-i identity_file] [-l limit]
          [-o ssh_option] [-P port] [-R num_requests] [-S program]
          [-s subsystem | sftp_server] host
       gsisftp [user@]host[:file ...]
       gsisftp [user@]host[:dir[/]]
       gsisftp -b batchfile [user@]host

# Install globus-proxy-utils
apt search globus-proxy-utils
sudo apt install globus-proxy-utils

# Install the OSG trusted certificates bundle,
# which is required to access the LSC clusters.
# This will add the certificates to a
# /etc/grid-security/certificates directory
sudo dpkg -i osg-ca-certs_1.31NEW-0_all.deb
```

Notes:
   * You should add repository for **old** version of Globus Toolkit 5.2.
   * You should add repository for old version of Ubuntu 14.04 `trusty`.
   * Distributions from new Globus Toolkit 6.0 Download web-page
   http://toolkit.globus.org/toolkit/downloads/6.0 do not work.
   There is no `gsissh` file in these files:
   `globus_toolkit-latest-x86_64-unknown-linux-gnu.tar.gz` and
   `globus-toolkit-repo_latest_all.deb`.
   So use GT 5.2 for Ubuntu 14.04 `trusty`.
   * IP-address should be whitelisted for the gsi-ssh on the server.
   Login only from whitelisted IP-address.   

---
### <a name="configure" />Configure GSI openssh client

You must have certificate `usercert.pem` and
encrypted private key `userkey.pem` to configure your GSI SSH client:
```shell
# Login to the computer with whitelisted IP-address.

# Create .globus/certificates directory in your $HOME
mkdir -p ~/.globus/certificates

# Move certificate and encrypted private key
# to the ~/.globus directory
mv Surname_usercert.pem ~/.globus/
mv Surname_userkey.pem ~/.globus/

# Rename them
cd ~/.globus/
mv Surname_usercert.pem usercert.pem
mv Surname_userkey.pem userkey.pem

# Change permissions to read only
chmod ugo-rwx usercert.pem userkey.pem
chmod u+r usercert.pem userkey.pem
ls -hal

# Copy all trusted certificates bundle from /etc/grid-security/certificates
# to your ~/.globus/certificates/ directory
cp /etc/grid-security/certificates/* ~/.globus/certificates/

# Create proxy connection for 24 hours. Must enter GRID pass phrase.
grid-proxy-init -debug -verify

    User Cert File: /home/username/.globus/usercert.pem
    User Key File: /home/username/.globus/userkey.pem
    
    Trusted CA Cert Dir: /home/username/.globus/certificates
    
    Output File: /tmp/x509up_u1003
    Your identity: /DC=by/DC=grid/O=uiip.bas-net.by/CN=Name Surname
    Enter GRID pass phrase for this identity:
    Creating proxy ...+++
    ................................+++
     Done
    Proxy Verify OK
    Your proxy is valid until: Fri Apr  5 08:47:41 2019

# Now try to connect via GSI SSH
gsissh int1-bb.cartesius.surfsara.nl -p 2222
                               SURFsara
                           Cartesius System

   Welcome to SURFsara,
   Some text here... doesn't matter.
   
   Consult https://userinfo.surfsara.nl/systems/cartesius
   for information on system usage.
Last login: Thu Apr  4 20:06:42 2019 from 80.94.171.57
********************************************************************************
* Some text here... doesn't matter.                                            *
********************************************************************************
* Questions? Please e-mail to helpdesk@surfsara.nl, or call 020-8001400        *
******************************************* last modified:  02-04-2019 10:30 ***
-bash-4.2$ 
```
