---
# Title, summary, and page position.
linktitle: dehyphen
summary: Documentation about dehyphen
weight: 30


# Page metadata.
title: dehyphen
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---

`dehyphen` is a Python package for the dehyphenation of broken text, e.g. extracted from a PDF.
It reconstructs the original continuous text by choosing the most probable way to join lines or paragraphs, and removes hyphens on the way.
The options are scored with the perplexity of character-based [Flair](https://github.com/flairNLP/flair) language models.

For this input

> die Bedeutung der finan-
>
> ziellen Interessen der Union

`dehyphen` joins the lines and removes the '-':

> die Bedeutung der **finanziellen** Interessen der Union

```bash
pip install dehyphen
```

Usage and the available models: <https://github.com/pd3f/dehyphen#readme>
