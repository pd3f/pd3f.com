---
title: Release notes
linktitle: Release notes
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 40
---

## v1.0.0

pd3f 1.0 is one Python package and one Docker image: LiteParse replaces Parsr, the former `pd3f-core` package is part of pd3f, and Redis, RQ and the OCR container are gone.

To upgrade from 0.6, wait until no job is queued or running and remove the old setup with its volumes, before you pull 1.0, which no longer has these Compose files:

```bash
docker compose -f docker-compose.yml -f docker-compose.prod.yml down -v   # in a checkout of the repository
docker compose down -v                                                    # with the docker-compose.yaml from this website
```

Then start 1.0 as described in [Installation](../installation/).
Old jobs are not taken over: delete `./data/pd3f-data-uploads/`, or `pd3f-data-uploads`, `pd3f-data-to-ocr` and `pd3f-data-cache` next to the website's Compose file.

- the web app listens on port 8000 instead of 1616
- the HTTP API moved to `/jobs`: `POST /jobs` instead of `POST /`, `GET /jobs/<id>` instead of `GET /update/<id>`, the outputs under `/jobs/<id>/` instead of `/files/`, and `DELETE /jobs/<id>`; see [Usage](../usage/#http-api)
- the option `fast` became `scorer=flair-fast`; the default scorer `none` decides by rules and the document's own spelling, without a language model
- what `experimental` did is the default: headers and footers once at the start and the end, footnotes at the end, paragraphs cut by a page break joined where the text goes on; `headers_footers`, `footnotes_in_place` and `page_breaks` change it
- tables always come as CSV; `tables`, `parsr_config` and `parsr_adjust_cleaner_config` are gone, `check_ocr` became `ocr` (`auto`, `force`, `skip`)
- `KEEP_RESULTS_HOURS` became `PD3F_JOB_TTL`; `RQ_REDIS_URL`, `REDIS_URL`, `WEB_CONCURRENCY`, `JOB_TIMEOUT`, `SENTRY_URL`, `DEMO`, `PRIVACY_URLS`, `MAX_UPLOAD_DISPLAY`, `POLL_INTERVAL`, the form field `in_browser`, the RQ dashboard and the `flask retry-failed` and `delete-failed` commands are gone
- the PyPI package `pd3f` carried pd3f-core up to 0.5.0; from 1.0.0 on it is this package: `pd3f.extract` returns a result for `pd3f.output.text` and `pd3f.output.tables` instead of the text and the tables
- each line end, page break and choice between copies of a running line is logged in `decisions.json`, and the web app marks the uncertain ones
- a command line tool, `pd3f`, and Markdown output with headings, lists and tables
- OCR runs only on the pages that need it; scans are straightened and turned upright first
- running headers and footers are recognized despite OCR's misreadings of their copies
- dehyphen 0.6 runs Flair's language models with numpy, so the image has no PyTorch and is about 700 MB
- the image runs natively on arm64, such as Apple Silicon
- a new web app: several uploads at once, the text next to the PDF, light and dark mode, operable by keyboard

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
