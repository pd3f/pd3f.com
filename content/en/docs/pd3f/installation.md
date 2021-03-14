---
# Title, summary, and page position.
linktitle: Installation
summary: Installation
weight: 5


# Page metadata.
title: Installation
date: "2021-03-08T00:00:00Z"
type: book  # Do not modify.
---

You need to setup [Docker](https://docs.docker.com/get-docker/), fetch the `pd3f` repository and a start script:

```bash
git clone https://github.com/pd3f/pd3f
cd pd3f
./dev.sh # or ./prod.sh
```

Adapt the different `docker-compose.yml` files for your needs.

The first time the `pd3f` starts it will download (and build) the Docker images.
Alltogether you need ~8 GB of space to store all the required components.


