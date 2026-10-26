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

You need [Docker](https://docs.docker.com/get-docker/) with Docker Compose.
Fetch the `pd3f` repository and run a start script:

```bash
git clone https://github.com/pd3f/pd3f
cd pd3f
./prod.sh # or ./dev.sh --build
```

`prod.sh` uses the published Docker images, `dev.sh --build` builds them from the checkout.
Adapt the `docker-compose*.yml` files to your needs; the [README](https://github.com/pd3f/pd3f#configuration) lists the environment variables.

The first start downloads about 10 GB of Docker images.

The published images are amd64 only, so on ARM machines such as Apple Silicon `./prod.sh` runs them emulated and a lot slower.
`./dev.sh --build` builds pd3f natively, only Parsr stays emulated.
