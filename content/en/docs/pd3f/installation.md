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

You need to setup [Docker](https://docs.docker.com/get-docker/).

You need the `docker-compose.yml` file of this repository. You can download it separately or just fetch the whole repository:

```bash
git clone https://github.com/pd3f/pd3f
```

Then go to the folder of this repository and run:

```bash
docker-compose up
```

The first time the `pd3f` starts it will download the Docker images.
You need to have ~8 GB of space to store all the software / data to run this.

