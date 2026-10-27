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

## Docker

The Docker image contains pd3f, its web app and everything OCR needs.
It is published for amd64 and arm64, so it also runs natively on Apple Silicon.

```bash
docker run -p 127.0.0.1:8000:8000 -v pd3f:/data pd3f/pd3f:1.0
```

Or with Docker Compose: save [`docker-compose.yaml`](/docker-compose.yaml) in a folder of its own and run `docker compose up` there.
Jobs and the language models then live in the folder `pd3f-data` next to it.

Then open <http://localhost:8000>.
The image is a download of about 250 MB, about 700 MB unpacked.
Its Tesseract has the data for German, English, French, Italian and Spanish; for OCR in another language, build an image on it that adds that language's package, such as `tesseract-ocr-rus`.
The web app has no authentication: keep it on localhost, or put a reverse proxy with access control in front, see [In production with nginx](../prod/).

## Python

```bash
pip install pd3f          # library and command line tool
pip install "pd3f[web]"   # with the web app
```

pd3f needs Python 3.11 or later.
OCR also needs Tesseract with the data of the document's language and unpaper, e.g. `apt install tesseract-ocr tesseract-ocr-deu unpaper`. Without Ghostscript, OCRmyPDF warns about each JPEG image it leaves as it is; the output is the same.
PDFs that have text need none of them.
