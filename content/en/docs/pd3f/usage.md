---
title: Usage
linktitle: Using pd3f
type: book
date: "2021-03-08T00:00:00Z"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 7
---

## Web app

Open <http://localhost:8000> and upload one or more PDFs.
The first job, or the one picked from the list, shows its steps while it runs, then the text next to the PDF.
The uncertain decisions are marked; each mark can be focused and shows the options.
Copy the text, or download it as text or Markdown, with the decision log, the tables as CSV and the OCRed PDF.

The browser remembers its own jobs.
The first job with a Flair scorer in a language downloads that scorer's two language models; `flair-fast` shares one pair for German, French and Italian.

## Command line

```bash
pd3f letter.pdf > letter.txt
pd3f letter.pdf --lang en --headers-footers drop > letter.txt
pd3f letter.pdf --scorer flair --out letter/   # text.txt, text.md, decisions.json; tables/*.csv and ocr.pdf if any
```

The main options:

- `--lang`: `de` (default), `en`, `es`, `fr` or `it`; another language has no Flair scorer and needs `--ocr-lang` for OCR
- `--scorer`: what decides the cases the rules leave open, `none` (default), `flair` or `flair-fast`, see [How it works](../how_it_works/)
- `--headers-footers`: `separate` (default: one copy at the start and the end of the text), `drop` or `keep` in place, which leaves paragraphs cut by a page break apart
- `--footnotes-in-place`: keep footnotes at the end of their page instead of the end of the text; a paragraph cut by a page break then stays apart there
- `--ocr`: `auto` (default: pages without usable text), `force` or `skip`
- `--out`: a new or empty directory for the outputs instead of the text on stdout

## Python

```python
from pd3f import extract, output

result = extract("letter.pdf", lang="de", scorer="none")
print(output.text(result))
```

## HTTP API

```python
import time

import requests

with open("letter.pdf", "rb") as f:
    job = requests.post("http://localhost:8000/jobs", files={"pdf": f}, data={"lang": "de"}).json()

while (status := requests.get(f"http://localhost:8000{job['url']}").json())["state"] in ("queued", "running"):
    time.sleep(1)
if status["state"] == "failed":
    raise SystemExit(status["error"])
print(requests.get(f"http://localhost:8000{job['url']}/text.txt").text)
```

`POST /jobs` takes the PDF as `pdf` and the options of the command line as form fields: `lang` (one of the five), `scorer`, `headers_footers`, `ocr`, `ocr_lang`, and `footnotes_in_place` and `page_breaks`, which switch on when present.
The outputs are files of the job: `text.txt`, `text.md`, `decisions.json`, `ocr.pdf`, `tables/<n>.csv`.
`DELETE /jobs/<id>` deletes a job and its files.
The [README](https://github.com/pd3f/pd3f#http-api) describes the status and the errors.

## Housekeeping

The web app keeps each job in a directory under `/data/jobs` in the container (`PD3F_JOBS`) and the scorers' models under `/data/flair`.
Jobs older than `PD3F_JOB_TTL` hours (24) are deleted when a new job is created.
`PD3F_WORKERS` (2) sets how many PDFs are extracted at the same time.
Jobs that were queued or running when the container stopped are marked failed at the next start; upload those PDFs again.
