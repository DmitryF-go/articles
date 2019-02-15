   - [Task](#task)
   - [Useful links](#links)
   - [Create request for certificate](#request)
   - [Check certificate](#check)

---
### <a name="task" />Task
   - Create request for personal Grid certificate.

Institution Name: United Institute of Informatics Problems (UIIP NASB).

Web site http://ca.grid.by doesn't function for now (2019.02.15).

Tested on Windows 10.

---
### <a name="links" />Useful links
   - [Configuration file](http://uiip.bas-net.by/ca/misc)
   - [Certification center UIIP NASB](http://uiip.bas-net.by/ca)
   - [BYGCA.pem](https://github.com/dCache/dcache-docker/blob/master/dcache/dcache/etc/grid-security/certificates/BYGCA.pem)
   - [Contents of current OSG CACert Distribution (version 1.78IGTFNEW)](https://repo.opensciencegrid.org/cadist)
   - [Операционный центр национальной грид-сети Республики Беларусь](http://noc.grid.basnet.by/index.php?n=Main.Ca)
   - [EUGridPMA Membership](https://www.eugridpma.org/members/membership)
   - [SEE-GRID Certification Authority](https://see-grid-ca.hellasgrid.gr/about)
   - [Certificate request generation via the command line and without configuration file](https://see-grid-ca.hellasgrid.gr/documents/certificate-requests)
   - [Небольшой рассказ про X.509](http://ca.grid.kiae.ru/RDIG/info/x509.html)

SEE-GRID Certification Authority: Nikos Nikoloutsakos <nikoloutsa at admin dot grnet dot gr> (Greece, GRNET)

As you can see, the authority member for Belarus is BYGCA, Alexander Shahk (shag at uiip dot bas-net dot by)

Now (2019.02.15) person who makes certificates: Ermak Dmitriy (dmierk at hep dot by), http://inp.bsu.by

---
### <a name="request" />Create request for certificate

 1. Install OpenSSL utility.
   - for Windows: [openssl-0.9.8h-1-setup.exe](https://sourceforge.net/projects/gnuwin32/)
     via http://gnuwin32.sourceforge.net/packages/openssl.htm
   - for Linux: openssl package included into your version of operating system

 2. Download configuration file `globus-user-ssl.conf` from
[certification center UIIP NASB](http://uiip.bas-net.by/ca/misc).
Edit config file and change default_bits from 1024 to 2048.

 3. Generate a request for Grid certificate:
```shell
# Generate file: usercert_request.pem
openssl req -new -config globus-user-ssl.conf -out usercert_request.pem -sha256

WARNING: can't open config file: /usr/local/ssl/openssl.cnf
Generating a 2048 bit RSA private key
....................................................+++
.........................+++
unable to write 'random state'
writing new private key to 'userkey.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
DomainComponent (by) [by]:by
DomainComponent (grid) [grid]:grid
Domain of your organization (e.g. uiip.bas-net.by) []:uiip.bas-net.by
Name (e.g., Francysk Skaryna) []:Foo Bar
```

 4. Two files are created: `usercert_request.pem` (request for certificate) and
`userkey.pem` (encrypted private key, hide it).
Verify created request PEM file:
```shell
# Verify request
openssl req -in usercert_request.pem -noout -text

WARNING: can't open config file: /usr/local/ssl/openssl.cnf
Unable to load config info from /usr/local/ssl/openssl.cnf
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: DC=by, DC=grid, O=uiip.bas-net.by, CN=Foo Bar
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:91:5f:2c:83:70:b9:e5:be:38:94:b6:28:f8:2f:
                    58:9c:b4:a3:73:ca:61:4b:89:bb:91:08:b6:23:bd:
                    9f:dd:a4:00:cc:20:09:3d:82:0c:7c:a8:49:54:6c:
                    99:c1:63:02:84:c5:ce:1b:7c:d7:3c:58:23:7b:13:
                    45:10:d9:2d:fa:31:09:79:5d:9d:c8:98:48:e2:0b:
                    60:54:6c:6b:00:3d:2a:59:29:ec:1d:1f:2e:cc:ae:
                    05:47:ea:82:48:fd:63:73:6d:13:ec:4b:f7:4d:00:
                    32:7c:79:2a:e7:64:f3:7b:47:ce:d0:97:92:6c:f0:
                    84:a3:ae:6b:51:82:a6:ee:a6:55:50:d1:5d:1b:c7:
                    7f:ad:e6:1d:e8:f7:5c:80:c8:a4:99:f5:ed:df:bf:
                    3b:d7:cc:c2:16:68:03:23:38:69:74:b4:a3:65:3a:
                    c4:86:72:64:f6:0e:89:f1:42:c7:1a:09:f4:24:bc:
                    87:a0:e4:97:30:6f:79:c5:22:91:b7:08:33:b3:0a:
                    b6:7a:e6:e1:d0:f7:e1:46:12:04:ab:f8:aa:60:c9:
                    7d:e5:53:69:f4:a4:1c:fe:98:0b:12:1c:d3:ac:b2:
                    93:23:be:6f:72:d7:a3:f6:58:0c:e7:6c:46:e9:bc:
                    50:6a:78:67:c3:c9:7c:59:ae:36:c0:08:e3:80:f4:
                    a5:2f
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha256WithRSAEncryption
         7a:54:85:b0:e3:01:dd:a8:31:92:2c:63:0b:fb:86:b5:a6:9f:
         7d:b0:1a:8b:80:da:8d:ff:6f:a9:1a:b6:59:83:88:1d:7e:bb:
         74:e2:38:34:82:82:a6:4a:26:65:82:d0:70:6e:94:89:31:ff:
         cc:60:5d:cf:f8:82:48:a2:1c:0e:f5:ce:13:49:23:86:93:0c:
         01:4b:8f:bc:b1:98:bd:20:fd:22:16:89:64:4b:f1:c9:0c:1e:
         26:39:64:24:75:22:7c:8a:8b:63:29:d9:96:d9:bd:a7:04:bf:
         5c:aa:41:84:6c:96:65:c0:e7:d3:d9:72:c1:10:26:8a:d2:c4:
         65:af:3e:06:61:71:d8:2a:aa:5a:0e:2d:9b:43:39:86:1c:82:
         84:7b:39:89:74:cd:60:e8:85:59:1f:c7:3c:3b:ba:54:5a:9f:
         30:50:77:ca:b6:b9:dc:e4:f7:86:6e:29:2a:30:98:0a:c3:ef:
         b0:96:09:f3:42:7d:52:0e:4e:46:75:42:e8:c9:76:ca:d9:a1:
         9a:26:fe:a7:2a:c3:37:65:3d:d2:c0:a6:8c:25:a6:6d:37:89:
         4a:c9:96:48:83:8e:cb:eb:26:9c:6e:5c:89:cb:a9:a1:f5:80:
         6b:43:35:47:ef:e8:e5:34:ed:50:6b:ee:72:79:25:a9:e7:1b:
         eb:8c:70:f6
```

Field "Subject" should be like this: `DC=by, DC=grid, O=uiip.bas-net.by, CN=Foo Bar`.

 5. Send request for certificate (file `usercert_request.pem`)
to the local request authority
with the following information:
```text
Foo Bar personal info:
1. Firstname Lastname: Foo Bar
2. Institution Name: United Institute of Informatics Problems (UIIP NASB)
3. Address: Surganova 6, 220012 Minsk, Belarus
4. Subdivision: Laboratory of Informatics
5. Position/Role: Software Engineer
6. E-mail: foobar167@gmail.com
7. Phone number: Office +375-17-225-96-19, Mobile: +375-29-225-12-46
8. Certificate request file "userkey.pem" is attached to the letter.
```
Do not send file `userkey.pem`. This is your private, secret key.

---
### <a name="check" />Check certificate

After receiving your certificate via e-mail (file `<Surname>_usercert.pem`),
verify this file:
```shell
# Have to be OK. Any other output means verification is wrong.
openssl verify -verbose -CAfile BYGCA.pem <Surname>_usercert.pem

WARNING: can't open config file: /usr/local/ssl/openssl.cnf
<Surname>_usercert.pem: OK
```
where file [BYGCA.pem](https://github.com/dCache/dcache-docker/blob/master/dcache/dcache/etc/grid-security/certificates/BYGCA.pem)
is a certificate authority (CA) file to verify your certificates.
