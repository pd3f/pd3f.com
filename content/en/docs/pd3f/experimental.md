---
title: Experimental Mode
linktitle: Experimental Mode
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 10
---

The experimental mode tries to redefine PDFs even further. 
In experimental mode, pd3f may throw certain parts of a PDF away, e.g., duplicate headers.
So the output needs to get checked manually.

## Features happen all the time

### Dehyphenation of Lines

Check if two lines can be joined by removing hyphens ('-').

### Reasonable Joining of Lines

Decide between adding a simple space (' ') or a new line ('\n') when joining lines.

## Experimental Mode

Use it with care.

### Reverse Page Break

Check if the last paragraph of a page und the first paragraph of the following page can be joined.

### Footnote to Endnotes

In order to join paragraphs (and reverse page breaks), detect footnotes and turn them into endnotes.
For now, the footnotes are pulled to the end of a file.

### Deduplication of Pager Header / Footer

If the header or the footer are the same for all pages, only display them once.
Headers are pulled to the start of the document and footer to the end.
Some heuristic based on the similarity of footers are used. (Jaccard distance for text, and compare overlapping shapes)

It's for OCRd PDFs hard to decide when a Header / Footer are duplicates.
The text in the header / footer is small the OCR text is faulty.

Special case for OCRd PDFs: Choose the Header / Footer with the best Flair score to display.
