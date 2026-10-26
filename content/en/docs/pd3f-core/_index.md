---
# Title, summary, and page position.
linktitle: pd3f-core
summary: Documentation about pd3f-core
weight: 20


# Page metadata.
title: pd3f-core
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---

`pd3f-core` is the Python package at the heart of pd3f.
It reconstructs the original continuous text from PDFs that are text-based or already OCRd.
[Parsr](https://github.com/axa-group/Parsr) chunks a PDF into lines and paragraphs, then [dehyphen](/docs/dehyphen/) joins them in the most probable way, judged by character-based language models.

```bash
pip install pd3f
```

- Installation and usage: <https://github.com/pd3f/pd3f-core#readme>
- API documentation: <https://pd3f.github.io/pd3f-core/>
