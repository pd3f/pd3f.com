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

`down -v` also removes the `cache` volume, so the first job downloads the language models again, and the OCR files of old jobs in the `to_ocr` volume.
Files of jobs from before the upgrade are not deleted automatically, remove them from `./data/pd3f-data-uploads/`.

If you run pd3f with the [`docker-compose.yaml`](/docker-compose.yaml) from this website, stop it with `docker compose down -v`, download the file again and start it with `docker compose pull && docker compose up`; files of old jobs stay in `pd3f-data-uploads` and `pd3f-data-to-ocr`.
Own Compose files need `RQ_REDIS_URL` (e.g. `redis://redis:6379`) on `web` and `worker`, and the worker has to run `rq worker --with-scheduler`.

- update to Python 3.13, Flask 3, RQ 2, Redis 8 and OCRmyPDF 17
- the worker runs the RQ scheduler, so jobs and their files are deleted automatically `KEEP_RESULTS_HOURS` after they finished or failed; `rq-scheduler` and the `rqscheduler` command are gone
- the Docker image uses the CPU build of PyTorch and is a lot smaller
- pd3f-core 0.5 and dehyphen 0.4 no longer depend on Flair, the language models are loaded with a restricted unpickler
- the RQ dashboard runs from the `pd3f/pd3f` image
- the `pd3f/pd3f` and `pd3f/pd3f-ocr` images are published for amd64 and arm64
- set `REDIS_URL` to use an external Redis
- fix: the OCR worker did not start with current Docker versions, so every job waited forever
- fix: flags such as `fast=false` were read as true
- fix: `example_api.py` waited forever on failed jobs
- fix: PDFs ending in `.PDF` waited forever for OCR
- fix: the result page kept polling and showed no download buttons for jobs without tables, and did not show a single table
- fix: text from the PDF was inserted into the result page as HTML
- fix: the web app could cut off the first lines of a job log
- fix: after a failed OCR, the result page showed an error on every poll once the files were deleted
- fix: invalid JSON in `parsr_config` or `parsr_adjust_cleaner_config` caused an internal server error instead of status 400
- `./prod.sh` publishes the web app only on 127.0.0.1, like the dashboard; for access from other machines, put a reverse proxy in front, see [In production with nginx](/docs/pd3f/prod/)
- `./dev.sh` publishes Flask's development server only on 127.0.0.1
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
