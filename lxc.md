# Criação dos container

>Variaveis:
>- container
>- size
>- host_ip
>- cpu
>- memory
```bash
$ lxc storage create <container>-drive btrfs size=<size>GB
$ lxc launch ubuntu:26.04 <container> \
  --storage <container>-drive \
  --config limits.cpu=<cpu> \
  --config limits.memory=<memory>GB

# Galera
$ lxc config device add <container> <container>-galera-11567 proxy listen=tcp:<host_ip>:11567 connect=tcp:127.0.0.1:11567
$ lxc config device add <container> <container>-galera-11568 proxy listen=tcp:<host_ip>:11568 connect=tcp:127.0.0.1:11568
$ lxc config device add <container> <container>-galera-11444 proxy listen=tcp:<host_ip>:11444 connect=tcp:127.0.0.1:11444

# ProFTPD
$ lxc config device add <container> <container>-ftp proxy listen=tcp:<host_ip>:11021 connect=tcp:127.0.0.1:21
$ lxc config device add <container> <container>-ftp-passive-1 proxy listen=tcp:<host_ip>:11022 connect=tcp:127.0.0.1:11022
$ lxc config device add <container> <container>-ftp-passive-2 proxy listen=tcp:<host_ip>:11023 connect=tcp:127.0.0.1:11023
$ lxc config device add <container> <container>-ftp-passive-3 proxy listen=tcp:<host_ip>:11024 connect=tcp:127.0.0.1:11024
$ lxc config device add <container> <container>-ftp-passive-4 proxy listen=tcp:<host_ip>:11025 connect=tcp:127.0.0.1:11025
$ lxc config device add <container> <container>-ftp-passive-5 proxy listen=tcp:<host_ip>:11026 connect=tcp:127.0.0.1:11026
$ lxc config device add <container> <container>-ftp-passive-6 proxy listen=tcp:<host_ip>:11027 connect=tcp:127.0.0.1:11027
$ lxc config device add <container> <container>-ftp-passive-7 proxy listen=tcp:<host_ip>:11028 connect=tcp:127.0.0.1:11028
$ lxc config device add <container> <container>-ftp-passive-8 proxy listen=tcp:<host_ip>:11029 connect=tcp:127.0.0.1:11029
```

