---
title: Usage
linktitle: Using pd3f
type: book
date: "2021-03-08T00:00:00Z"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7
---


## Using the GUI

The first time you upload a PDF, `pd3f` will download some large languages models.
After it's finished access the Web-based GUI at <http://localhost:1616>.

After uploading a PDF you will get redirected to a web page displaying progress / results of the job.

## Using the API

```python
import time

import requests

files = {'pdf': ('test.pdf', open('/dir/test.pdf', 'rb'))}
response = requests.post('http://localhost:1616', files=files, data={'lang': 'de'})
id = response.json()['id']

while True:
    r = requests.get(f"http://localhost:1616/update/{id}")
    j = r.json()
    if 'text' in j:
        break
    print('waiting...')
    time.sleep(1)
print(j['text'])
```

Post params:
 - `lang`: set the language (options: 'de', 'en', 'es', 'fr')
 - `fast`: whether to check for tables (default: False)
 - `tables`: whether to check for tables (default: False)
 - `experimental`: whether to extract text in experimental mode (footnotes to endnotes, depuplicate page header / footer) (default: False)
 - `check_ocr`: whether to check first if all pages were OCRd (default: True, cannot be modified in GUI)

You have to poll for `/update/<uuid>` to keep up with the progress. The responding JSON tells you about the status of the processing job.

Fields:
 - `log`: always present, text output from the job.
 - `text` and `tables` and `filename`: only present when the job finished successfully
 - `position`: present if on waiting list, returns position as integer
 - `running`: present if job is running
 - `failed`: present if job has failed

### Scaling

You can also run more parsr workers with this:

```bash
docker-compose up --scale worker=3
```

To increase the frontend workers, adapt the env `WEB_CONCURRENCY` in `docker-compose.yml`:

```yml
WEB_CONCURRENCY=2
```

You may as well create a new `docker-compose.yml` to override certain settings. Take a look at [`docker-compose.prod.yml`](./docker-compose.prod.yml)

```bash
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up --scale worker=2
```

### House Keeping

Docker uses three volumes:

- `pd3f-data-uploads`: input & output files, mapped to `./data/pd3f-data-uploads/`
- `pd3f-data-cache`: internal volume, storing data so you don't have to download model files over and over again
- `pd3f-data-to-ocr`: internal volume, temporary location for PDFs to get OCRd. Files get deleted but logs get kept.

Results are kept for 24 hours per default. But no files get deleted automatically (only the results in the queue (e.g. the extracted text)).

Run this command from time to time to schedule jobs in order to delete files in `pd3f-data-uploads` and `pd3f-data-to-ocr`.

```bash
docker-compose run --rm worker rqscheduler --host redis --burst
```
