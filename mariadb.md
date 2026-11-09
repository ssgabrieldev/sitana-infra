# Commands

>Variables:
- pass
```bash
$ apt install -y mariadb-server mariadb-client
$ systemctl enable --now mariadb
$ mariadb-secure-installation # ENTER, Y, N, Yx4

# allow galera to execute rsync
$ ln -s /etc/apparmor.d/mariadbd /etc/apparmor.d/disable/mariadbd
$ ls -l /etc/apparmor.d/disable/mariadbd
$ /etc/init.d/apparmor restart
$ apparmor_parser -R /etc/apparmor.d/mariadbd

# change mariadb root password
$ mariadb
mariadb> alter user 'root'@'localhost' identified by '<pass>';
