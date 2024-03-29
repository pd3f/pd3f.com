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

Python package for **dehyphenation of broken text**, i.e., extracted from a PDF. Mainly for the German but works for other languages as well.

Check out the [source code on GitHub](https://github.com/jfilter/dehyphen).

`dehyphen` tries to reconstruct the original continuous text by choosing the most probably way to join lines or paragraphs (and remove hyphens).
Several options are getting scored by calculating the [perplexity](https://en.wikipedia.org/wiki/Perplexity#Perplexity_per_word) of text, using [Flair](https://github.com/flairNLP/flair)'s character-based [language models](https://machinelearningmastery.com/statistical-language-modeling-and-neural-language-models/).
Based on these scores, the best fitting option is taken to guess the original text.


## An Example

For this input

> die Bedeutung der finan-
>
> ziellen Interessen der Union

`dehyphen` joines the lines and removes the '-'.

> die Bedeutung der **finanziellen** Interessen der Union

But in this example

> Auch andere EU-
>
> Staaten, wie bspw. Polen,

the lines are also joined bu the hyphen is kept (becaus it's part of the word).

> Auch andere **EU-Staaten**, wie bspw. Polen,


## Installation

```bash
pip install dehyphen
```

or

```bash
poetry add dehyphen
```

## Usage

```python
from dehyphen import FlairScorer

scorer = FlairScorer(lang="de")
```

You need to set `lang` to `de` for German, `en` for English, `es` for Spanish, etc. Otherwise, a multi-language-model will be chosen as the default. [See this section in the source code for more models](https://github.com/flairNLP/flair/blob/8c09e62d9a5a3c227b9ca0fb9f214de9620d4ca0/flair/embeddings/token.py#L431) (but omit the "-backwards" and "-forwards" as specified by Flair). [Some are described here](https://github.com/flairNLP/flair/blob/master/resources/docs/embeddings/FLAIR_EMBEDDINGS.md) and [there is another repo with some more models](https://github.com/flairNLP/flair-lms).

To speed up computations, choose a `-fast` language model from Flair. However, there are currently only a few.
There is for instance a multi-language one named `multi-v0` that contains English, German, French and others.
Unfortunately, there is no fast German-only model right now.

Using CUDA (with a GPU) dramatically improves performance.

### 1. remove hyphens from the end of a line (within paragraphs)

```python
# returns cleaned paragraph
scorer.dehyphen(special_format)
```

The input text has to be in a special format. Paragraphs should be seperated by two newlines characters (`\n\n`). Line should be end with a single newline `\n`. Several helper functions exists to transform the data into the required format.

### 2. join paragraphs, e.g., to reverse a page break

```python
# returns the joined paragraphs if the language model thinks there were split, otherwise `None`
scorer.is_split_paragraph(paragraph_1, paragraph_2)
```

## Example

```python
from dehyphen import FlairScorer

scorer = FlairScorer(lang="de")

some_german_text = """Zwar wird durch die Einführung eines eigenen Strafgesetzes die Bedeutung der finan-
ziellen Interessen der Union gewiss unterstrichen, dennoch erscheint die Aufspaltung
des strafrechtlichen Vermögensschutzes zweifelhaft, insbesondere soweit es densel-
ben Schutzgegenstand, nämlich die vermögensrelevanten Interessen der Union be-
trifft. Zum einen wird es den Normunterworfenen ohne Not erschwert, die zu befolgen-
den Strafgesetze zu erfassen. Zum anderen ergeben sich potentielle Auslegungsdif-

ferenzen durch die Verwendung teilweise abweichender Terminologie (finanzielle In-
teressen vs. Vermögen). Schließlich wird der Schutz besagter Interessen ohnedies
bislang innerhalb des StGB gewährleistet. Daher empfiehlt es sich u.E., sämtliche Re-
gelungen des RegE in das StGB zu integrieren, soweit entsprechende Neuregelungen
überhaupt erforderlich sind. Hierdurch wird sich auch eine klarere Trennung von Straf-
recht und Verwaltungsrecht erreichen lassen.

Das Erfolgsverständnis entspricht daher eher dem wesentlich weiteren Betrugsbegriff
bspw. des US-amerikanischen Rechts (Federal Law bspw. Fraud, Defraud, Wire-
Fraud, Bank-Fraud, 18.U.S.C. §1341 ff.(2016)) , die teilweise auch ganz auf einen
Schaden verzichten. Fraud erfasst auch viele untreue- und unterschlagungsähnliche
Verhaltensweisen sowie betrügerische Verfügungen als solche. Auch andere EU-
Staaten, wie bspw. Polen, liegen im Hinblick auf den Erfolg näher bei der Richtlinie
als bei der deutschen Schadensdogmatik.
"""

special_format = text_to_format(some_german_text)
fixed_hyphens = scorer.dehyphen(special_format)

# checks if two paragraphs can be joined, useful to, e.g., reverse page breaks.
joined_paragraph = scorer.is_split_paragraph(fixed_hyphens[:2])

print(joined_paragraph)
```
**Output text**:

Zwar wird durch die Einführung eines eigenen Strafgesetzes die Bedeutung der finanziellen Interessen der Union gewiss unterstrichen, dennoch erscheint die Aufspaltung des strafrechtlichen Vermögensschutzes zweifelhaft, insbesondere soweit es denselben Schutzgegenstand, nämlich die vermögensrelevanten Interessen der Union betrifft. Zum einen wird es den Norm unterworfenen ohne Not erschwert, die zubefolgenden Strafgesetze zu erfassen. Zum anderen ergeben sich potentielle **Auslegungsdifferenzen** durch die Verwendung teilweise abweichender Terminologie (finanzielle Interessen vs. Vermögen). Schließlich wird der Schutz besagter Interessen ohnediesbislang innerhalb des StGB gewährleistet. Daher empfiehlt es sich u.E., sämtliche Regelungen des RegE in das StGB zu integrieren, soweit entsprechende Neuregelungenüberhaupt erforderlich sind. Hierdurch wird sich auch eine klarere Trennung von Strafrecht und Verwaltungsrecht erreichen lassen.

*Hyphens are removed, paragraphs are joined along the word **Auslegungsdifferenzen**.*

```python
print(fixed_hyphens[-1])
```
**Output text**:

Das Erfolgsverständnis entspricht daher eher dem wesentlich weiteren Betrugsbegriff bspw. des US-amerikanischen Rechts (Federal Law bspw. Fraud, Defraud, **Wire-Fraud**, Bank-Fraud, 18.U.S.C. §1341 ff.(2016)), die teilweise auch ganz auf einen Schaden verzichten. Fraud erfasst auch viele untreue- und unterschlagungsähnliche Verhaltensweisen sowie betrügerische Verfügungen als solche. Auch andere **EU-Staaten**, wie bspw. Polen, liegen im Hinblick auf den Erfolg näher bei der Richtlinie als bei der deutschen Schadensdogmatik und Verwaltungsrecht erreichen lassen.

***EU-Staaten** & **Wire-Fraud** are not dehyphenized.*
