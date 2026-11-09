# Commands
>Variables:
>- folder
>- user
```bash
$ apt install -y \
    proftpd-core \
    proftpd-mod-crypto 
$ service proftpd restart

# garant access to nginx
$ chmod 755 /srv/ftp

$ vim /etc/proftpd/ftpd.group # file:1
$ vim /etc/proftpd/proftpd.conf  # file:2
$ vim /etc/proftpd/tls.conf # file:3
$ vim /etc/proftpd/modules.conf # file:4
$ mkdir -p /srv/ftp/<folder>
$ chown -R 60000:nogroup /srv/ftp/<folder>
$ ftpasswd --passwd --file=/etc/proftpd/ftpd.passwd --name=<user> --uid=60000 --gid=65534 --home=/srv/ftp/<folder> --shell=/bin/false
$ service proftpd restart
```

# Files

## 1: /etc/proftpd/ftpd.group
```
nogroup:x:65534:
```

## 2: /etc/proftpd/proftpd.conf
Change or add next lines:

>Variables:
>- domain
```
ServerName                      "<domain>"

DefaultRoot                     ~

RequireValidShell               off

PassivePorts                  11022 11029

AuthOrder                       mod_auth_file.c
AuthUserFile /etc/proftpd/ftpd.passwd
AuthGroupFile /etc/proftpd/ftpd.group

Include /etc/proftpd/tls.conf
```
## 3: /etc/proftpd/tls.conf
Change or add next lines:
>Variables
>- domain:
```
TLSEngine                               on
TLSLog                                  /var/log/proftpd/tls.log
TLSProtocol                             SSLv23

#TLSRSACertificateFile                   /etc/ssl/certs/proftpd.crt
#TLSRSACertificateKeyFile                /etc/ssl/private/proftpd.key
TLSECCertificateFile /root/keys/<domain>/fullchain.pem
TLSECCertificateKeyFile /root/keys/<domain>/privkey.pem

TLSOptions                      EnableDiags NoSessionReuseRequired

TLSVerifyClient                         off

TLSRequired                             on
```

## 4: /etc/proftpd/modules.conf
Change or add next lines:
```
LoadModule mod_tls.c
```
