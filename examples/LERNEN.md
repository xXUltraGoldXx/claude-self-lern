# Lerndatei (Beispiel)

Nur anhängen, nie überschreiben. Alle Einträge unten sind erfunden.

## 2026-01-05 09:12 · demo-shop · Migration auf neue DB-Version
- Schief gelaufen: Migration lief gegen die Test-DB, nicht gegen die lokale Kopie; Daten waren danach verändert.
- Besser machen: Vor jeder Migration Verbindungsziel ausgeben und bestätigen.
- Gut gelaufen: Rollback-Skript war vorher geschrieben und funktionierte.
- Merke: Vor schreibenden DB-Befehlen immer das Verbindungsziel prüfen und ausgeben.

## 2026-01-07 16:40 · demo-api · Flaky Test im CI
- Schief gelaufen: Zwei Stunden Fehlersuche im Produktivcode, Ursache war eine feste Uhrzeit im Test.
- Besser machen: Bei sporadisch roten Tests zuerst nach Zeit, Zufall und Reihenfolge-Abhängigkeit suchen.
- Gut gelaufen: Fix war ein Einzeiler plus Regressionstest.
- Merke: Sporadisch rote Tests zuerst auf Uhrzeit, Zufall und Testreihenfolge prüfen.

## 2026-01-12 11:05 · demo-frontend · Styling nach Update kaputt
- Schief gelaufen: Annahme, das Build-Tool sei schuld; tatsächlich fehlte ein Cache-Reset im Browser.
- Besser machen: Erst mit hartem Reload und leerem Cache gegenprüfen, dann Tool verdächtigen.
- Gut gelaufen: Screenshot vorher/nachher machte das Problem schnell greifbar.
- Merke: Bei Darstellungsfehlern nach Deploy zuerst Cache umgehen, dann erst Code ändern.
