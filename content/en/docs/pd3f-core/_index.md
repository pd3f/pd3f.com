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

`pd3f-core` was the Python package at the heart of pd3f 0.x: it rebuilt the continuous text from the output of [Parsr](https://github.com/axa-group/Parsr).
Since pd3f 1.0 it is part of [pd3f](/docs/pd3f/), and the PyPI package `pd3f` contains the whole pipeline:

```bash
pip install pd3f
```

```python
from pd3f import extract, output

print(output.text(extract("letter.pdf")))
```

The repository [pd3f-core](https://github.com/pd3f/pd3f-core) is archived and keeps the code of the 0.x versions; `pip install "pd3f<1"` installs the last of them.
