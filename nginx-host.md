# Commands

>Variables:
>- container
>- domain
```bash
$ vim /etc/nginx/sites-available/<domain>
$ ln -s /etc/nginx/sites-available/<domain> /etc/nginx/sites-enabled/<domain>
$ nginx -s reload
$ certbot certonly --webroot -w /var/www/html -d <domain> --preferred-challenges http
$ vim /etc/nginx/sites-available/<domain>
$ nginx -s reload

# ProFTPD
$ lxc exec <container> -- mkdir -p /root/keys/<domain>
$ lxc file push /etc/letsencrypt/live/<domain>/fullchain.pem <container>/root/keys/<domain>/
$ lxc file push /etc/letsencrypt/live/<domain>/privkey.pem <container>/root/keys/<domain>/
$ lxc exec adv-ao-prod -- mv /root/keys/<domain>/fullchain1.pem /root/keys/<domain>/fullchain.pem
$ lxc exec adv-ao-prod -- mv /root/keys/<domain>/privkey1.pem /root/keys/<domain>/privkey.pem
```

# Files

## /etc/nginx/sites-available/<domain>
>Variables:
>- domain
>- container-ip
```
server {
    listen 80;
    listen [::]:80;
    server_name <domain>;

    location /.well-known {
        root /var/www/html;
    }

    return 301 https://$http_host$request_uri;
}
server {
    listen [::]:443 ssl;
    listen 443 ssl;
    server_name <domain>;

    ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;

    location / {
        proxy_pass http://<container-ip>:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_redirect off;
        proxy_buffer_size          128k;
        proxy_buffers              4 256k;
        proxy_busy_buffers_size    256k;
    }

    # phpmyadmin
    location = /phpmyadmin {
        return 301 /phpmyadmin/;
    }
    location ^~ /phpmyadmin/ {
        proxy_pass http://<container-ip>:80/;
        proxy_set_header Host phpmyadmin;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_redirect ~^(/.*)$ off;
    }

    location /.well-known {
        root /var/www/html;
    }
}
```
