---
title: Running pd3f in production with nginx
linktitle: In production with nginx
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 30
---

The web app has no authentication: anyone who reaches it can upload PDFs, and anyone with the link of a job can read and delete it.
Publish it only on 127.0.0.1, as [`docker-compose.yaml`](/docker-compose.yaml) does, and put a reverse proxy with access control in front.
An example for nginx with Basic Auth:

```nginx
server {
    server_name pdf.example.org;
    # PD3F_MAX_MB, 100 by default
    client_max_body_size 100M;

    auth_basic "pd3f";
    auth_basic_user_file /etc/nginx/pd3f.htpasswd;

    location / {
        proxy_pass http://127.0.0.1:8000;
    }
}
```

Run one container: it starts `PD3F_WORKERS` extraction processes itself.
A second container on the same jobs directory would mark the running jobs of the first as interrupted when it starts.
