---
title: FAQ
linktitle: FAQ
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 20
---

### What happened to Parsr, Redis and the OCR container?

pd3f 1.0 reads PDFs with LiteParse in its own process instead of Parsr, which is no longer maintained and recommends LiteParse itself.
Parsing a page went from about two seconds, with Parsr emulated on Apple Silicon, to a hundredth of a second.
Extractions run in a process pool of the web app, so there is no Redis and no separate worker, and OCRmyPDF runs as a library on the pages that need it.
One image of about 700 MB replaces five containers from four images and 10 GB.

### Does pd3f need a GPU or PyTorch?

No. The Flair scorers run Flair's language models with numpy, which gives the same scores as PyTorch at the same speed on a CPU.
The default scorer `none` needs no language model at all.

### Does pd3f send my PDFs anywhere?

No. Everything runs on your machine.
The Flair scorers download their language models from Flair's servers on first use, and nothing else goes out.

### Why is Flair not the default?

On a sample of German statements, the rules and the spelling the document uses elsewhere settle the hyphens as well as Flair does.
Flair still helps with line ends in short lines, such as addresses and lists, and costs seconds per page.

### What does pd3f not do?

It does not rebuild tables cell by cell beyond what LiteParse finds, it keeps LiteParse's reading order, which can mix up the columns of some multi-column pages, and it has no authentication for its web app.
It was made for German letters and official documents; English, Spanish, French and Italian work as well.
