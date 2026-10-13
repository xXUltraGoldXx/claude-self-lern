---
name: self-lern
description: Self Lern – vor jeder Aufgabe die Merksätze der Lerndatei beachten, nach jeder abgeschlossenen Aufgabe eine kurze Rückschau (was ist schief gelaufen, was besser machen, was gut gelaufen) per `self-lern eintrag` anhängen. Die Datei wird nie überschrieben.
---

<!-- EN summary: Before each task, heed the rules ("Merksätze") that the SessionStart hook injects from the learning file. After each finished task, append a short, honest retrospective (what went wrong / what to do better / what went well / one rule) with `self-lern eintrag`. Append-only, concrete, no secrets or customer data. -->

# Self Lern

Ziel: Fehler nicht wiederholen und schneller zu Lösungen kommen.

Die Lerndatei liegt standardmäßig unter `~/.claude/self-lern/LERNEN.md` (änderbar über die Umgebungsvariable `SELF_LERN_DATEI`).

## Vorher (bei jeder Aufgabe)
- Die Merksätze kommen beim Sitzungsstart automatisch per SessionStart-Hook (`self-lern hook`) in den Kontext – beachten.
- Passt ein Thema zur Aufgabe: `self-lern lesen <stichwort>` für die vollständigen Einträge.

## Nachher (nach jeder abgeschlossenen Aufgabe/Sitzung)
Kurz und ehrlich drei Fragen beantworten und anhängen:

```
self-lern eintrag --thema "<kurz>" \
  --schief "<was ist schief gelaufen>" \
  --besser "<was hätte man besser machen können>" \
  --gut "<was ist gut gelaufen>" \
  --merke "<ein Satz als Regel für nächstes Mal>"
```

Regeln:
- Nur anhängen. Nie mit Write/Edit/`>` bearbeiten, nie Einträge löschen oder umschreiben. Korrekturen als neuer Eintrag. (Optional schützt `chattr +a` die Datei auf Dateisystemebene.)
- Konkret statt allgemein ("Stylesheet lieferte 302 wegen Cache" statt "CSS-Problem").
- Keine Geheimnisse, keine Kundennamen oder -daten.
- Nichts Neues gelernt (Routine ohne Auffälligkeit) → kein Eintrag, keine Floskeln.
- Merksatz = eine prüfbare Handlungsregel; er erscheint künftig in jedem Sitzungsstart.
- Arbeiten Hintergrund-Agenten oder Unteragenten mit: Rückschau in deren Rückmeldung schreiben; die steuernde Sitzung entscheidet, ob ein Eintrag entsteht (verhindert Doppel-Einträge).
