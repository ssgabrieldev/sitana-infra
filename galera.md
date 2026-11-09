# Commands
```bash
$ vim /etc/mysql/conf.d/galera.cnf # file:1
$ service mariadb stop

# if primary node
$ galera_new_cluster

# if not primary node
$ service mariadb stop
$ service mariadb start
```

# Files

## 1: /etc/mysql/conf.d/galera.cnf
>Variables:
>- primary_server_ip
>- current_server_ip
>- other_server_ip
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
wsrep_cluster_address="gcomm://<primary_server_ip>:11567,<other_server_ip>:11567"
wsrep_sst_receive_address=<current_server_ip>:11444

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Galera Node Configuration
wsrep_node_address="<current_server_ip>"
wsrep_node_name="adv-angola-prod"
wsrep_provider_options="pc.recovery=TRUE;base_port=11567;ist.recv_addr=<current_server_ip>:11568;ist.recv_bind=0.0.0.0"
```
