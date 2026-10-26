---
title: Release notes
linktitle: Release notes
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 40
---

## v0.6.x

To upgrade from 0.5, wait until the RQ dashboard shows no queued or running jobs, then stop everything and remove the volumes, because RQ 2 can't read the results of the old jobs:

```bash
docker compose -f docker-compose.yml -f docker-compose.prod.yml down -v
git pull
docker compose -f docker-compose.yml -f docker-compose.prod.yml pull
./prod.sh -d
```

`down -v` also removes the `cache` volume, so the language models get downloaded again.
Files of jobs from before the upgrade are not deleted automatically, remove them from `./data/pd3f-data-uploads/`.

- update to Python 3.13, Flask 3, RQ 2, Redis 8 and OCRmyPDF 17
- the worker runs the RQ scheduler, so files of finished jobs are deleted automatically after `KEEP_RESULTS_HOURS`; `rq-scheduler` and the `rqscheduler` command are gone
- the Docker image uses the CPU build of PyTorch and is a lot smaller
- pd3f-core 0.5 and dehyphen 0.4 no longer depend on Flair, the language models are loaded with a restricted unpickler
- the RQ dashboard runs from the `pd3f/pd3f` image
- set `REDIS_URL` to use an external Redis
- fix: the OCR worker did not start with current Docker versions, so every job waited forever
- fix: flags such as `fast=false` were read as true
- fix: `example_api.py` waited forever on failed jobs
- fix: PDFs ending in `.PDF` waited forever for OCR
- fix: the result page kept polling and showed no download buttons for jobs without tables, and did not show a single table
- fix: text from the PDF was inserted into the result page as HTML
- fix: the web app could cut off the first lines of a job log
- `docker-compose.yml` uses the published `pd3f/pd3f-ocr` image
- table CSVs no longer contain an index column

## v0.5.x

- make parsr config customizable. check out <https://github.com/pd3f/pd3f/blob/master/example_api.py> and <https://pd3f.github.io/pd3f-core/export.html#pd3f.export.extract>

## v0.4.x

- improve Docker-Compose setup
- add Italian support
- update to Parsr v1.2.2, OCRmyPdf v11.6.2
- make RQ-Dashboard v0.6.3 work by pinning RQ to v1.5.2
- add optional timeout for rq jobs to mark started jobs that wont finish as failed

## v0.3.x

- first public release
