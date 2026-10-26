---
title: Usage
linktitle: Using pd3f
type: book
date: "2021-03-08T00:00:00Z"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7
---

## Using the GUI

Access the Web-based GUI at <http://localhost:1616>.
After uploading a PDF you get redirected to a page displaying the progress and the results of the job.
The first job downloads the language models, which takes a while.

## Using the API

```python
import sys
import time

import requests

with open("/dir/test.pdf", "rb") as f:
    response = requests.post(
        "http://localhost:1616", files={"pdf": ("test.pdf", f)}, data={"lang": "de"}
    )
job_id = response.json()["id"]

while True:
    j = requests.get(f"http://localhost:1616/update/{job_id}").json()
    if "text" in j:
        break
    if "failed" in j:
        sys.exit(j["log"])
    print("waiting...")
    time.sleep(1)
print(j["text"])
```

POST parameters:

- `pdf`: the PDF file
- `lang`: the language, one of `de`, `en`, `es`, `fr`, `it`
- `fast`: use smaller, faster but less accurate language models (default: false)
- `tables`: extract tables (default: false)
- `experimental`: deduplicate page headers and footers and remove page numbers from them, move footnotes to the end and join paragraphs split across pages (default: false)
- `check_ocr`: OCR pages without text first (default: true, cannot be changed in the GUI)
- `parsr_config`, `parsr_adjust_cleaner_config`: JSON to change the configuration of Parsr, see [`example_api.py`](https://github.com/pd3f/pd3f/blob/master/example_api.py) and the [pd3f-core documentation](https://pd3f.github.io/pd3f-core/export.html#pd3f.export.extract)

Flags accept `1`, `true`, `on` and `yes`, or `0`, `false`, `off`, `no` and an empty value, in any case.
Other values are rejected with status 400.

Poll `/update/<id>` to follow the progress. The JSON response contains:

- `log`: always present, text output from the job
- `text`, `tables` and `filename`: only present when the job finished successfully; `tables` is a list of CSV strings, or `null` without `tables`
- `position`: present if the job is waiting, its position in the queue
- `running`: present if the job is running
- `failed`: present if the job has failed

Once `/update/<id>` has returned the text, the results are also available as files: `/files/<filename>.txt` and `/files/<filename>_tables.zip`, with `filename` from that response.

### Scaling

Run more workers to process several PDFs at once:

```bash
./prod.sh --scale worker=3
```

To increase the number of web server processes, adapt the environment variable `WEB_CONCURRENCY` in [`docker-compose.prod.yml`](https://github.com/pd3f/pd3f/blob/master/docker-compose.prod.yml).

### House Keeping

pd3f stores data in four places:

- `./data/pd3f-data-uploads/`: input and output files
- the `cache` volume: language models and cached scores, so they are not downloaded or computed again
- the `to_ocr` volume: PDFs waiting for OCR; the OCR worker removes them, the OCRed PDFs and the logs stay until the files of the job get deleted
- inside the `parsr` container: Parsr never deletes the files of its jobs, a few MB per PDF

The worker deletes the files of a job `KEEP_RESULTS_HOURS` after it finished (24 in `docker-compose.yml`), the queue keeps the results for 24 hours.
Except after a failed OCR, the files of failed jobs are kept so you can retry or delete them:

```bash
docker compose exec worker flask retry-failed
docker compose exec worker flask delete-failed
```

To free the space Parsr uses, recreate its container when no job is running:

```bash
docker compose up -d --force-recreate parsr
```
