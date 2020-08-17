+++
widget = "blank"
headless = true  # This file represents a page section.
weight = 30  # Order that this section will appear.

title = 'Übersicht'
subtitle = ''

[design]
  # Choose how many columns the section has. Valid values: 1 or 2.
  columns = "1"
+++

*Eine einfachere und längere Einführung zu pd3f gibt es auf der Seite des [Prototype Fund](https://prototypefund.de).*

pd3f erkennt automatisiert Text auf gescannte PDFs mit [OCRmyPDF](https://github.com/jbarlow83/OCRmyPDF) (Tesseract) und extrahiert Tabellen mit [Camelot](https://github.com/camelot-dev/camelot) und [Tabula](https://github.com/tabulapdf/tabula).
Es baut auf der Ausgabe von [Parsr](https://github.com/axa-group/Parsr) auf.
Parsr erkennt Hierarchien von Text und teilt den Text in Wörter, Zeilen und Absätze auf.

Obwohl Parsr etwas Struktur in die PDF-Datei bringt, ist der Text immer noch zerstümmelt, z. B. sind Wörter durch Bindestriche getrennt.
Das zugrundeliegende Python-Paket [pd3f-core](https://github.com/pd3f/pd3f-core) versucht, den ursprünglichen Fließtext zu rekonstruieren, indem es Bindestriche, neue Zeilen und/oder Leerzeichen entfernt.
Es verwendet maschinelles Lernen mit [Sprachmodellen (Language Models)](https://machinelearningmastery.com/statistical-language-modeling-and-neural-language-models/), um zu erraten, wie der ursprüngliche Text aussah.

pd3f ist besonders nützlich für Sprachen mit langen Wörtern wie im Deutschem.
Es wurde hauptsächlich entwickelt, um deutsche Briefe und offizielle Dokumente zu analysieren.
Neben Deutsch unterstützt pd3f auch Englisch, Spanisch und Französisch.
Weitere Sprachen werden später hinzugefügt.

pd3f enthält eine webbasierte GUI und einen [Flask](https://flask.palletsprojects.com/)-basierten Microservice (API).
Eine Demo finden Sie unter [demo.pd3f.com](https://demo.pd3f.com).

Eine systematische Evaluierung von pd3f erfolgt im September 2020.


![](/media/flow.jpg)
