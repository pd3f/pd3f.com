---
title: How it works
linktitle: How it works
type: book
date: "2026-10-27T00:00:00Z"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 10
---

## Steps

1. [LiteParse](https://github.com/run-llama/liteparse) reads the words of each page with their boxes and fonts.
2. Pages without a text layer, or with more than a tenth of their glyphs mapped to no character, go through [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF) and Tesseract. The pages are straightened, cleaned and turned upright first; the OCRed PDF is read again.
3. Lines that repeat at the top or bottom of enough pages become running headers and footers. OCR's misreadings of the copies are tolerated. Page numbers are removed unless kept in place.
4. Lines become paragraphs, headings, list items and tables, by their spacing, their fonts, where they end and the text blocks LiteParse finds.
5. The paragraphs at the end of a page in a smaller font, from the first one starting with a number, become footnotes.
6. Each line end, each page break a paragraph may run on across and each choice between differing copies of a running line is decided and logged.

## Decisions

| decision | options |
|---|---|
| hyphen at the end of a line | join (`Rechts-` / `staat` → `Rechtsstaat`), keep the hyphen (`EU-Staaten`), hyphen and space (`Bundes- und Landesrecht`) |
| line end without a hyphen | new line or space |
| paragraph cut by a page break | keep apart or join |
| copy of a running header or footer | which one stays |

Most decisions are settled before any language model: by the layout (the next word would have fit on the line), by fixed rules (a conjunction after the hyphen, an uppercase start), or by the spelling the document uses elsewhere.
What remains goes to the scorer:

- `none` (default): join the hyphen, continue the line, keep paragraphs on different pages apart, keep the first copy.
- `flair`: [Flair](https://github.com/flairNLP/flair)'s forward and backward character language models judge which option reads most plausibly, via [dehyphen](/docs/dehyphen/). Seconds per page; downloads two models of 73 MB each for German on first use.
- `flair-fast`: Flair's smaller models, several times faster.

On a sample of 14 German statements on draft laws, the rules and the document's vocabulary rendered all 376 line-final hyphens right, so `none` is the default.
The Flair scorers help with the line ends of short lines that no rule settles, such as some addresses and lists.

## The decision log

`decisions.json` lists these decisions with their options, the text each would give, the one chosen and what settled it.
A decision is uncertain when the default made it, or when the scorer's best option was barely ahead.
The web app marks the uncertain ones in the text.
