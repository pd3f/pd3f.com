+++
widget = "blank"
headless = true  # This file represents a page section.
weight = 30  # Order that this section will appear.

title = 'Overview'
subtitle = ''

[design]
  # Choose how many columns the section has. Valid values: 1 or 2.
  columns = "1"
+++

*A longer introduction to pd3f 0.x in German [on the site of the Prototype Fund](https://archive.demoweek.prototypefund.de/runde7/projects/06-ddd.html).*

pd3f reads PDFs with [LiteParse](https://github.com/run-llama/liteparse) and runs OCR on scanned pages with [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF) (Tesseract).
It then rebuilds the continuous text: it joins lines into paragraphs, rejoins words hyphenated at the end of a line, moves running headers, footers and page numbers out of the text and collects the footnotes at the end.
Fixed rules and the spelling the document uses elsewhere settle most of these decisions.
Optionally, the character-based language models of [Flair](https://github.com/flairNLP/flair) settle the rest.

pd3f is especially useful for languages with long words such as German.
It was mainly developed to parse German letters and official documents.
Besides German, pd3f supports English, Spanish, French and Italian.

pd3f is a Python library, a command line tool and a web app with an HTTP API, packaged as one Docker image.

pd3f is licensed under the [GNU Affero General Public License v3.0](https://www.gnu.org/licenses/agpl-3.0.en.html) and is looking for contributors. Write an email to [hi@jfilter.de](mailto:hi@jfilter.de) if you want to cooperate.

### Recorded online talk about pd3f 0.x

{{< youtube iaIgTx49a6s >}}
