---
title: Documentation
type: book  # Do not modify.
---

pd3f extracts the continuous text of a PDF: it rebuilds paragraphs from lines, rejoins hyphenated words, moves running headers and footers out of the text, collects footnotes and runs OCR on scans.
It is a Python library, a command line tool and a small web app, packaged as one Docker image.

- [pd3f](/docs/pd3f/): installation, usage and operation
- [dehyphen](/docs/dehyphen/): the package that scores the uncertain joins with Flair's language models
- [pd3f-core](/docs/pd3f-core/): the former post-processing package, merged into pd3f 1.0
