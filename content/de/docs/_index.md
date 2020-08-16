---
title: Dokumentation
type: book  # Do not modify.
---

Willkommen zu der Dokumentation zu pd3f.
Die vollständige Dokumentation gibt es nur in [Englisch](/docs).
Es folgt nur eine kurze Einführung, welche jedoch den Umgang mit der Kommando-Zeile (macOS: Terminal, Windows: Power-Shell) erfordert.

## Installation

Installieren Sie zunächst [Docker](https://docs.docker.com/get-docker/) auf Ihrem Computer und starten Sie Docker.

Erstellen Sie einen Ordern Ihrer Wahl und speichern folgende Datei in diesem Order ab: [docker-compose.yaml](/docker-compose.yaml).

Starten Sie pd3f in dem Sie in dem Order folgendes ausführen:

```bash
docker-compose up
```

Anschließend werden ungefähr 8 GB an Dateien heruntergeladen. Dann steht unter [localhost:1616](http://localhost:1616) eine lokale Webseite bereit, mit der pd3f zu bedienen ist.


### Neue Order

Nach der Bedienung von pd3f erscheinen drei neue Order.

- `pd3f-data-uploads`: Ein- und Ausgabe-Dateien werden hier gespeichert
- `pd3f-data-cache`: Zwischenspeicher damit große Dateien, die für das Maschinelle Lernen benötigt werden, nicht immer wieder neu geladen werden müssen
- `pd3f-data-to-ocr`: temporärer Ordner
