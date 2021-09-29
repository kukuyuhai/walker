---
title: nginx基本配置backup
date: 2021-9-18 16:18
tags: nginx https
---

#### 常规 http 配置

```
server {
    listen       80;
    server_name  test.chinadep.com;

    root /opt/project/daep-fe;
    index index.html index.htm;

    charset utf-8;
    access_log  /opt/project/logs/nginx/daep_access_nginx.log;
    error_log /opt/project/logs/nginx/daep_error_nginx.log;

    client_max_body_size 55m;

    gzip on;
    gzip_min_length 50k;
    gzip_comp_level 2;
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
    gzip_vary on;

    location / {
        try_files $uri $uri/ /index.html;
        index index.html index.htm;
        autoindex on;
    }

    location /daep {
        # rewrite  ^/api/?(.*)$ /$1 break;
        proxy_pass   http://10.101.12.23:8093;
        proxy_set_header Host $http_host;
        add_header backendIP $upstream_addr;
        add_header backendCode $upstream_status;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

```

#### https nginx 配置

```
server {
        listen 80;
        server_name hyadmin.chinadep.com;
        rewrite ^(.*)$  https://$host$1 permanent;
}

server {
        listen 443;
        server_name hyadmin.chinadep.com;
        ssl on;
        ssl_certificate /etc/nginx/sslkey/_.chinadep.com_bundle.crt;
        ssl_certificate_key /etc/nginx/sslkey/_.chinadep.com.key;
        ssl_session_timeout 60m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers RC4:HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;
        root /opt/project/daep-admin-fe;
        gzip on;
        gzip_http_version 1.0;
        gzip_disable "MSIE [1-6].";
        gzip_types text/plain application/x-javascript text/css text/javascript application/javascript;
        access_log /var/log/daep-admin/access_all.log;
        error_log /var/log/daep-admin/error_all.log;
        gzip_comp_level 3;
        charset utf-8;

        location /daep-admin {
            proxy_pass http://127.0.0.1:8091;
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
}
```
