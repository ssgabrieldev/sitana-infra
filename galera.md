# Commands
```bash
$ vim /etc/mysql/conf.d/galera.cnf
$ service mariadb stop

# if primary node
$ galera_new_cluster

# if not primary node
$ service mariadb stop
$ service mariadb start
```

# Files

## /etc/mysql/conf.d/galera.cnf
>Variables:
>- primary-server-ip
>- current-server-ip
>- other-server-ip
```
[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

# Galera Provider Configuration
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so

# Galera Cluster Configuration
wsrep_cluster_name="adv-angola-cluster"
wsrep_cluster_address="gcomm://<primary-server-ip>:11567,<other-server-ip>:11567"
wsrep_sst_receive_address=<current-server-ip>:11444

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Galera Node Configuration
wsrep_node_address="<current-server-ip>"
wsrep_node_name="adv-angola-prod"
wsrep_provider_options="pc.recovery=TRUE;base_port=11567;ist.recv_addr=<current-server-ip>:11568;ist.recv_bind=0.0.0.0"
```
