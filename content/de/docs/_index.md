---
title: Dokumentation
type: book  # Do not modify.
---

Willkommen zu der Dokumentation zu pd3f.
Die vollständige Dokumentation gibt es nur in [Englisch](/docs).
Es folgt nur eine kurze Einführung, welche jedoch den Umgang mit der Kommando-Zeile (macOS: Terminal, Windows: PowerShell) erfordert.

## Installation

Installieren Sie zunächst [Docker](https://docs.docker.com/get-docker/) auf Ihrem Computer und starten Sie Docker.

Erstellen Sie einen Ordner Ihrer Wahl und speichern Sie folgende Datei in diesem Ordner ab: [docker-compose.yaml](/docker-compose.yaml).

Starten Sie pd3f, indem Sie in dem Ordner Folgendes ausführen:

```bash
docker compose up
```

Beim ersten Start wird das Docker-Image heruntergeladen, etwa 250 MB. Dann steht unter [localhost:8000](http://localhost:8000) eine lokale Webseite bereit, mit der pd3f zu bedienen ist.
pd3f läuft auch auf Rechnern mit ARM-Prozessor, etwa Apple Silicon, ohne Emulation.
Für ein Update laden Sie die Datei erneut herunter, führen `docker compose pull` aus und beachten die [Release Notes](/docs/pd3f/release_notes/).

## Bedienung

Laden Sie ein oder mehrere PDFs hoch.
Für das erste PDF, oder das in der Liste gewählte, zeigt pd3f, woran es gerade arbeitet, und dann den Text neben dem PDF.
Unsichere Entscheidungen, etwa ob ein Zeilenende ein Leerzeichen oder ein Zeilenumbruch ist, sind markiert.
Den Text können Sie kopieren oder als Text oder Markdown herunterladen, dazu die Tabellen als CSV und das PDF mit Texterkennung.

Ergebnisse, die älter als 24 Stunden sind, werden beim nächsten Hochladen gelöscht.

### Neuer Ordner

Neben der Datei `docker-compose.yaml` entsteht der Ordner `pd3f-data`:

- `pd3f-data/jobs`: die hochgeladenen PDFs und ihre Ergebnisse
- `pd3f-data/flair`: die Sprachmodelle von Flair, wenn Sie unter „Scorer for uncertain decisions“ `flair` oder `flair-fast` wählen; sie werden nur einmal heruntergeladen
