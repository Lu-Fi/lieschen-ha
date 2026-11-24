# Lieschen Telefonassistent für Home Assistant

Home-Assistant-Integration für **Lieschen Müller**, einen KI-Telefonassistenten an der Fritzbox (Asterisk + Gemini).
Über die Sprachbefehl-Schnittstelle von Lieschen startet Home Assistant Gespräche, die an einem Telefon im Haus beginnen,
zum Beispiel per Alexa: „Alexa, Headset mit Max Handy verbinden“.

Voraussetzung ist eine laufende Lieschen-Installation mit eingeschalteten Sprachbefehlen und einem Schlüssel
(Lieschen > Einstellungen > Anbindungen > Sprachbefehle). Ohne Lieschen tut diese Integration nichts.

## Was sie kann

- **Schalter** je Zeile `Name | Gerät | Ziel`:
  - *Verbinden*: das Telefon im Haus klingelt, nach dem Abheben wählt Lieschen das Ziel (Nummer, Kontakt oder Benutzer), ohne KI.
  - *KI* (Ziel `KI`): nach dem Abheben meldet sich die KI, fragt, mit wem sie verbinden soll, und verbindet. Das kostet KI-Minuten.
  - Modus **Taster** (Standard): der Schalter bleibt eine Mindestzeit an (Standard 3 s) und geht dann aus, damit Alexa-Routinen ihn sicher erkennen.
    Modus **Gespräch**: an, solange das Gespräch läuft; aus = auflegen.
- **Sensor „Anruf“** mit dem Zustand des letzten Gesprächs: `bereit`, `klingelt`, `abgenommen`, `ki`, `verbunden`, `beendet`,
  `nicht_erreicht` (mit Grund), `fehler`. Attribute u. a. Gerät, Ziel und welches Telefon abgehoben hat.
- **Ereignis** `lieschen_call` bei jeder Zustandsänderung.
- **Aktionen** `lieschen.verbinden`, `lieschen.assistent`, `lieschen.auflegen` (mit Antwort, z. B. der `call_id`).
- Auswahllisten für Geräte und Ziele kommen live aus Lieschen; Gerätenamen (z. B. „Headsets“ = mehrere Telefone, wer abhebt, wird verbunden)
  legt man in Lieschen an.

Die Verbindung ist lokal (`local_push`, Server-Sent Events), es gibt keinen Cloud-Dienst.

## Installation

**HACS:** HACS > Benutzerdefinierte Repositories > `https://github.com/Lu-Fi/lieschen-ha`, Kategorie *Integration*,
dann „Lieschen Telefonassistent“ herunterladen und Home Assistant neu starten.

**Ohne HACS:** den Ordner `custom_components/lieschen` nach `/config/custom_components/` kopieren
(oder aus Lieschen: `curl -s http://<lieschen>:8091/ha/lieschen.tar.gz | tar xz -C /config`) und neu starten.

## Einrichtung

Einstellungen > Geräte & Dienste > Integration hinzufügen > „Lieschen Telefonassistent“: Adresse von Lieschen
(z. B. `http://192.168.1.10:8091`) und den Schlüssel eintragen. Schalter, Modus und Mindestzeit stehen danach in den Optionen.

## Stand und Grenzen

- Getestet mit Home Assistant 2026.10 und 2026.11, Version 0.1.0.
- Oberfläche und Texte bisher nur auf Deutsch (die englische Übersetzung enthält noch die deutschen Texte).
- Lieschen selbst ist ein eigenes Projekt; diese Integration ist nur die Anbindung an Home Assistant.

## Tests

```
pip install pytest-homeassistant-custom-component
pytest
```
