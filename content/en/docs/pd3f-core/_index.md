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


`pd3f-core` is Python package to **reconstruct** the original **continuous text** from **PDFs** with language models.
`pd3f-core` assumes your PDF is either text-based or already OCRd.
`pd3f-core` is at the heart of [pd3f](https://github.com/pd3f/pd3f): a full Docker-based text extraction pipeline (including OCR).

`pd3f-core` first uses [Parsr](https://github.com/axa-group/Parsr) to chunk PDFs into lines and paragraphs.
Then, it uses the Python package [dehyphen](https://github.com/jfilter/dehyphen) to reconstruct the paragraphs in the most probable way.
The probability is derived by calculating the [perplexity](https://en.wikipedia.org/wiki/Perplexity) with [Flair](https://github.com/flairNLP/flair)'s character-based [language models](https://machinelearningmastery.com/statistical-language-modeling-and-neural-language-models/).
Unnecessary hyphens are removed, space or new lines are kept or dropt depending on the surround words.

It's mainly developed for German but should work with other languages as well.
The project is still in an early stage.
Expect rough edges and rapid changes.

## API Documentation

API Documentation of pd3f-core: <https://pd3f.github.io/pd3f-core/index.html>

And check out the repository on GitHub for more information: <https://github.com/pd3f/pd3f-core>
