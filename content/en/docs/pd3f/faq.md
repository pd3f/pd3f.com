---
title: FAQ
linktitle: FAQ
type: book
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 20
---

### Parsr can also do OCR, why do use OCRmyPDF?

OCRmyPDF uses various image preprocessing to improve the image quality.
This improves the output of Tesseract.
Parsr uses raw Tesseract so the results are worse.

### Parsr can also extract text, why is your tool needed?

The text output of Parsr is scrambled, i.e. hyphens are not removed.
`pd3f` improves the overall text quality by reconstructing the original text with language models.
See [pdddf](https://github.com/pd3f/pdddf) for details.

Overall Parsr is a great tool, but it still has rough edges.
`pd3f` improves the output with various (opinionated) hand-crafted rules.
`pd3f` mainly focuses on formal letters and official documents for now.
Based on this assumption we can simplify certain things.
It was developed mainly for German documents but it should work for other languages as well.
