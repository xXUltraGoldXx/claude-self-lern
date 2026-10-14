# claude-self-lern

Ein kleiner Claude-Code-Skill mit Hilfsskript: Claude führt eine **nur erweiterbare Lerndatei**. Nach jeder abgeschlossenen Aufgabe entsteht eine kurze Rückschau (was lief schief, was besser, was gut, eine Merkregel). Die Merkregeln werden bei jedem Sitzungsstart automatisch in den Kontext geladen – so wiederholt Claude Fehler seltener.

## Installation über den Plugin-Marktplatz (empfohlen)

```
/plugin marketplace add xXUltraGoldXx/claude-plugins
/plugin install self-lern@ultragold
```

Updates holt `/plugin marketplace update ultragold`. Die manuelle Installation unten bleibt als Alternative.

## English in short

`claude-self-lern` gives Claude Code a persistent, append-only "lessons learned" file. A small Python script (`self-lern`) appends entries, searches them, and prints a SessionStart-hook text that injects the latest one-line rules into every new session. Only the Python standard library is needed (Linux/macOS). The file location is configurable via `SELF_LERN_DATEI` (default `~/.claude/self-lern/LERNEN.md`). Docs below are in German.

## Inhalt

| Datei | Zweck |
|---|---|
| `self-lern` | Skript: `eintrag`, `lesen`, `hook`, `selftest` |
| `skill/SKILL.md` | Der Claude-Code-Skill |
| `examples/LERNEN.md` | Beispiel-Lerndatei (erfundene Einträge) |
| `LICENSE` | MIT |

## Installation

Voraussetzung: Python 3, Claude Code.

1. Skript installieren:
   ```bash
   mkdir -p ~/.local/bin
   cp self-lern ~/.local/bin/self-lern
   chmod +x ~/.local/bin/self-lern
   ```
   `~/.local/bin` muss im `PATH` liegen.
2. Skill installieren:
   ```bash
   mkdir -p ~/.claude/skills/self-lern
   cp skill/SKILL.md ~/.claude/skills/self-lern/SKILL.md
   ```
3. SessionStart-Hook in `~/.claude/settings.json` eintragen (bestehende `hooks` zusammenführen, nicht ersetzen). Pfad zu `self-lern` ggf. absolut angeben:
   ```json
   {
     "hooks": {
       "SessionStart": [
         {
           "hooks": [
             { "type": "command", "command": "self-lern hook" }
           ]
         }
       ]
     }
   }
   ```
4. Optional: eigene Lerndatei-Position über die Umgebungsvariable festlegen, z. B. in der Shell-Konfiguration:
   ```bash
   export SELF_LERN_DATEI="$HOME/.claude/self-lern/LERNEN.md"
   ```
5. Optional (nur Linux, braucht root): Datei auf Dateisystemebene append-only machen, damit nichts überschrieben oder gelöscht werden kann:
   ```bash
   touch ~/.claude/self-lern/LERNEN.md   # Datei muss existieren
   sudo chattr +a ~/.claude/self-lern/LERNEN.md
   ```
   Aufheben (nur bewusst!): `sudo chattr -a <datei>`. Ohne `chattr` funktioniert alles genauso; der Skill verbietet Überschreiben ohnehin per Regel.
6. Prüfen: `self-lern selftest` gibt am Ende `ok` aus (nutzt eine temporäre Datei, deine echte Lerndatei bleibt unberührt).

## Nutzung

```bash
# Eintrag anhängen (alle fünf Felder Pflicht)
self-lern eintrag --thema "Flaky Test im CI" \
  --schief "Zwei Stunden im Produktivcode gesucht, Ursache war eine feste Uhrzeit im Test." \
  --besser "Bei sporadisch roten Tests zuerst Zeit und Zufall prüfen." \
  --gut "Fix war ein Einzeiler plus Regressionstest." \
  --merke "Sporadisch rote Tests zuerst auf Uhrzeit, Zufall und Testreihenfolge prüfen."

# Alles lesen oder nach Stichwort filtern
self-lern lesen
self-lern lesen test

# Text für den SessionStart-Hook (die letzten 40 Merksätze)
self-lern hook
```

Claude ruft `self-lern eintrag` nach jeder abgeschlossenen Aufgabe selbst auf, wenn der Skill geladen ist (siehe `skill/SKILL.md`). Ein Beispiel für eine fertige Datei liegt in `examples/LERNEN.md`; zum Ausprobieren:

```bash
SELF_LERN_DATEI=examples/LERNEN.md ./self-lern hook
```

### Beispiel-Ausgabe von `self-lern hook`

```
SELF LERN aktiv (Skill self-lern, Datei /home/du/.claude/self-lern/LERNEN.md):
1. Vor einer Aufgabe: Merksaetze unten beachten; bei passendem Thema `self-lern lesen <stichwort>` fuer Details.
2. Nach jeder abgeschlossenen Aufgabe kurze Rueckschau und anhaengen: `self-lern eintrag ...`
Merksaetze (neueste zuletzt):
- Vor schreibenden DB-Befehlen immer das Verbindungsziel prüfen und ausgeben. (2026-01-05, Migration auf neue DB-Version)
- Sporadisch rote Tests zuerst auf Uhrzeit, Zufall und Testreihenfolge prüfen. (2026-01-07, Flaky Test im CI)
- Bei Darstellungsfehlern nach Deploy zuerst Cache umgehen, dann erst Code ändern. (2026-01-12, Styling nach Update kaputt)
```

## Hinweise

- Einträge sind einzeilig pro Feld; Zeilenumbrüche werden zu Leerzeichen.
- Keine Geheimnisse und keine Kundendaten in die Lerndatei schreiben – sie wird bei jedem Start in den Kontext geladen.
- Das Skript benötigt nur die Python-Standardbibliothek (`fcntl` → Linux/macOS, nicht Windows).

## Lizenz

MIT, siehe `LICENSE`.
