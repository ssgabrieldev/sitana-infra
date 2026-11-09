# Commands

>Variables:
>- domain
```bash
$ apt install -y nginx 
$ mkdir -p /srv/webhosts/conf.d && mkdir -p /srv/webhosts/log
$ vim /etc/nginx/sites-available/hosting
$ ln -s /etc/nginx/sites-available/hosting /etc/nginx/sites-enabled/hosting
$ vim /etc/nginx/snippets/server.conf

# if php
$ apt install -y \
  php \
  php-cli \
  php-fpm \
  php-mysql \
  php-curl \
  php-gd \
  php-mbstring \
  php-xml \
  php-zip \
  php-intl \
  php-bcmath \
  php-soap
$ vim /etc/nginx/php_params
$ vim /etc/nginx/php_hosting.conf

$ vim /srv/webhosts/<domain>.conf

# if phpmyadmin
$ vim /etc/nginx/sites-available/phpmyadmin
$ ln -s /etc/nginx/sites-available/phpmyadmin /etc/nginx/sites-enabled/phpmyadmin

$ nginx -s reload
```

# Files

## /etc/nginx/sites-available/hosting
```nginx
include /srv/webhosts/*.conf;
```

## /etc/nginx/sites-available/phpmyadmin
```nginx
server {
    listen 80;
    listen [::]:80;
    server_name phpmyadmin;

    root /srv/phpmyadmin;

    include /etc/nginx/php_hosting.conf;

    index index.html index.htm index.php;

    location / {
        index index.html index.htm index.php;
        try_files $uri $uri/ /index.php?$args;
    }
}
```

## /etc/nginx/php_params
```nginx
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

fastcgi_param   QUERY_STRING            $query_string;
fastcgi_param   REQUEST_METHOD          $request_method;
fastcgi_param   CONTENT_TYPE            $content_type;
fastcgi_param   CONTENT_LENGTH          $content_length;

fastcgi_param   SCRIPT_FILENAME         $request_filename;
fastcgi_param   SCRIPT_NAME             $fastcgi_script_name;
fastcgi_param   REQUEST_URI             $request_uri;
fastcgi_param   DOCUMENT_URI            $document_uri;
fastcgi_param   DOCUMENT_ROOT           $document_root;
fastcgi_param   SERVER_PROTOCOL         $server_protocol;

fastcgi_param   GATEWAY_INTERFACE       CGI/1.1;
fastcgi_param   SERVER_SOFTWARE         nginx/$nginx_version;

fastcgi_param   REMOTE_ADDR             $remote_addr;
fastcgi_param   REMOTE_PORT             $remote_port;
fastcgi_param   SERVER_ADDR             $server_addr;
fastcgi_param   SERVER_PORT             $server_port;
fastcgi_param   SERVER_NAME             $server_name;

fastcgi_param   HTTPS                   $https if_not_empty;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param   REDIRECT_STATUS         200;

# include geoip_fastcgi_params;
```

## /etc/nginx/snippets/server.conf
```nginx
#add_header X-Frame-Options "SAMEORIGIN";
```

## /etc/nginx/php_hosting.conf
```nginx
location ~ /\.(?!well-known) {
    deny all;
    access_log off;
    log_not_found off;
}

location ~ \.swp$ {
    deny all;
}

location ~ .*wp-config.* {
    deny all;
}

location ~ ^/wp-content/.*\.(php|phps)$ {
    #allow 88.99.241.88;
    allow 195.201.171.244;
    allow 127.0.0.1;
    deny all;
}

location = /xmlrpc.php {
    deny all;
    access_log off;
    error_log off;
}

location ~ \.php$ {
    auth_basic off;
    try_files $uri =404;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_index index.php;
    #fastcgi_pass unix:/var/run/php7.2-fpm-hosting.sock;
    fastcgi_pass unix:/var/run/php/php8.5-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_buffers 16 16k;
    fastcgi_buffer_size 32k;
    fastcgi_read_timeout 600;
    include php_params;
}
```

## /srv/webhosts/<domain>.conf
>Variables:
>- domain
>- project-folder
```nginx
server {
    listen 80;
    listen [::]:80;
    server_name <domain>;

    root /srv/ftp/<project-folder>/web;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    include /etc/nginx/snippets/server.conf;

    # if php
    include /etc/nginx/php_hosting.conf;
}
```
