#### Sync local Postgres database with Digital Ocean droplet

```bash
#! /bin/bash
ssh root@107.107.107.107 'cd /home/deployer/quickdraw/current/ && pg_dump -Fc quickdraw_production > latest.dump'
scp root@107.107.107.107:/home/deployer/quickdraw/current/latest.dump ~/dropbox/code/retrograde
pg_restore --verbose --clean --no-acl --no-owner -j 2 -h localhost -d quickdraw_development latest.dump
ssh root@107.107.107.107 'cd /home/deployer/quickdraw/current/ && rm latest.dump'
rm latest.dump
```

#### Default Nginx file

```
upstream app {
    # Path to Unicorn SOCK file, as defined previously
    server unix:/home/deployer/quickdraw/shared/sockets/unicorn.sock fail_timeout=0;
}

server {

    listen 80;

    # Application root, as defined previously
    root /home/deployer/quickdraw/current/public;

    # Make site accessible from http://localhost/
    server_name localhost;

    try_files $uri/index.html $uri @app;

    access_log /var/log/nginx/quickdraw_access.log combined;
    error_log /var/log/nginx/quickdraw_error.log;

    location @app {
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://app;
        # proxy_set_header   X-Forwarded-Proto https;  # <-- don't need this if you're not running SSL
    }

    error_page 500 502 503 504 /500.html;
    client_max_body_size 4G;
    keepalive_timeout 10;
}
```
