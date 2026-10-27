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

*Eine längere Einführung zu pd3f 0.x gibt es in einem [Blog-Post auf der Seite des Prototype Fund](https://archive.demoweek.prototypefund.de/runde7/projects/06-ddd.html).*

pd3f liest PDFs mit [LiteParse](https://github.com/run-llama/liteparse) und erkennt den Text gescannter Seiten mit [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF) (Tesseract).
Dann rekonstruiert es den Fließtext: Es fügt Zeilen zu Absätzen zusammen, setzt am Zeilenende getrennte Wörter wieder zusammen, nimmt Kopf- und Fußzeilen samt Seitenzahlen aus dem Text und sammelt die Fußnoten am Ende.
Die meisten dieser Entscheidungen treffen feste Regeln und die Schreibweise, die das Dokument an anderer Stelle verwendet.
Den Rest können auf Wunsch die zeichenbasierten Sprachmodelle von [Flair](https://github.com/flairNLP/flair) entscheiden.

pd3f ist besonders nützlich für Sprachen mit langen Wörtern wie das Deutsche.
Es wurde hauptsächlich entwickelt, um deutsche Briefe und offizielle Dokumente zu bearbeiten.
Neben Deutsch unterstützt pd3f auch Englisch, Spanisch, Französisch und Italienisch.

pd3f ist eine Python-Bibliothek, ein Kommandozeilen-Werkzeug und eine Web-App mit HTTP-API, zusammen in einem Docker-Image.

pd3f steht unter der [GNU Affero General Public License v3.0](https://www.gnu.org/licenses/agpl-3.0.de.html).

### Online-Vortrag über pd3f 0.x (in Englisch)

{{< youtube iaIgTx49a6s >}}
