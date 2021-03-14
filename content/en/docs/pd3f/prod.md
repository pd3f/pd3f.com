---
title: Running pd3f in production with nginx
linktitle: In production with nginx
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 30
---


An example config for nginx to run in conjunction with [`docker-compose.prod.yml`](./docker-compose.prod.yml):

```nginx
# prevent guessing of file names / URLs
limit_req_zone $binary_remote_addr zone=limitfiles:10m rate=1r/s;

server {
    server_name changeme.com;
    client_max_body_size 50M;

    if ($request_method !~ ^(GET|HEAD|POST)$ )
    {
        return 405;
    }

    location /files/ {
        limit_req zone=limitfiles burst=2 nodelay;

        add_header Content-disposition "attachment";
        alias /var/pd3f/data/pd3f-data-uploads/;
    }

    location ~ /(update|result)/ {
        limit_req zone=limitfiles burst=2 nodelay;

        proxy_pass http://127.0.0.1:1616;
    }

    location / {
        # simple frontend caching
        expires 10m;
        add_header Cache-Control "public";

        proxy_pass http://127.0.0.1:1616;
    }

    # restrict access to dashboard to IP address (or subnet) + protect with Basic Auth
    location /dashboard/ {
        limit_req zone=limitfiles burst=20 nodelay;

        allow xx.xx.xx.xx;
        deny all;

        auth_basic "Private Area";
        auth_basic_user_file /path/to/.htpasswd;

        proxy_pass http://127.0.0.1:9181;
    }
}
```

Make sure set to set the correct permission to let nginx serve the static files (in `/var/pd3f/data/pd3f-data-uploads/`).

## Common Problems

If there are problems with rq (the queue), just remove the image:

```
docker rm root_redis_1
```

This will clean all data in redis.
