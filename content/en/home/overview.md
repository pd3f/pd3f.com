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

*A longer introduction to pd3f in German [on the site of the Prototype Fund](https://demoweek.prototypefund.de/projects/06-ddd.html).*

pd3f can OCR scanned PDFs with [OCRmyPDF](https://github.com/jbarlow83/OCRmyPDF) (Tesseract) and extracts tables with [Camelot](https://github.com/camelot-dev/camelot) and [Tabula](https://github.com/tabulapdf/tabula).
It's built upon the output of [Parsr](https://github.com/axa-group/Parsr).
Parsr detects hierarchies of text and splits the text into words, lines and paragraphs.

Even though Parsr brings some structure to the PDF, the text is still scrambled, i.e., due to hyphens.
The underlying Python package [pd3f-core](https://github.com/pd3f/pd3f-core) tries to reconstruct the original continuous text by removing hyphens, new lines and/or spaces.
It uses [language models](https://machinelearningmastery.com/statistical-language-modeling-and-neural-language-models/) to guess how the original text looked like.

pd3f is especially useful for languages with long words such as German.
It was mainly developed to parse German letters and official documents.
Besides German pd3f supports English, Spanish and French.
More languages will be added later.

pd3f includes a Web-based GUI and a [Flask](https://flask.palletsprojects.com/)-based microservice (API).
You can find a demo at [demo.pd3f.com](https://demo.pd3f.com).

A more systematic evaluation of pd3f will follow in September 2020.

pd3f is licensed under the [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html) and is looking for contributors. Write an email to [hi@jfilter.de](mailto:hi@jfilter.de) if want to cooperate.

![](/media/flow.jpg)

### Recorded Online Talk about pd3f

{{< youtube iaIgTx49a6s >}}
