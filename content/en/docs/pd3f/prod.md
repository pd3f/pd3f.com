---
title: Running pd3f in Production with Nginx
linktitle: In Production with Nginx
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 30
---


An example config for Nginx to run in conjunction with [`docker-compose.prod.yml`](./docker-compose.prod.yml):

```nginx
limit_req_zone $binary_remote_addr zone=limitfiles:10m rate=1r/s;
proxy_cache_path /var/nginx/cache keys_zone=pd3fcache:1m inactive=1m max_size=10M;

server {
    server_name demo.pd3f.com;
    client_max_body_size 50M;

    if ($request_method !~ ^(GET|HEAD|POST)$ )
    {
        return 405;
    }

    # prevent guessing of file names
    location /files/ {
        limit_req zone=limitfiles burst=2 nodelay;
        alias /var/pd3f/pd3f-data-uploads/;
        add_header Content-disposition "attachment";
    }

    location /update/ {
        proxy_pass http://127.0.0.1:1616;
    }

    # simple caching
    location / {
        proxy_cache pd3fcache;
        expires 10m;
        add_header Cache-Control "public";

        proxy_pass http://127.0.0.1:1616;
    }

    # restrict access to a IP / Subnet + protect with Basic Auth
    location /dashboard/ {
        allow xx.xx.xx.xx;
        deny all;

        auth_basic "Private Area";
        auth_basic_user_file /path/to/.htpasswd;

        proxy_pass http://127.0.0.1:9181;
}
```

Make sure set to set the correct permission to let Nginx serve the static files (in `/var/pd3f/pd3f-data-uploads/`).

## Common Problems

If there are problems with rq (the queue), just remove the image:

```
docker rm root_redis_1
```

This will clean all data in redis.
