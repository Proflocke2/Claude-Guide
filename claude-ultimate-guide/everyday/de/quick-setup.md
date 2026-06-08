# ⚡ Quick Setup — In 30 Minuten startklar

Drei Schritte, die den Unterschied zwischen "netter Chatbot" und "echter Assistent" ausmachen. Kein technisches Wissen nötig.

## Schritt 1 — Claude herunterladen & registrieren

**Browser (keine Installation):**
Geh auf [claude.ai](https://claude.ai) und erstelle ein kostenloses Konto mit E-Mail oder Google-Login.

**Desktop-App (empfohlen):**
```
claude.ai → oben rechts auf dein Profilbild → "Desktop App herunterladen"
→ .dmg (Mac) oder .exe (Windows) installieren → einloggen
```

**Mobil:**
Claude App im App Store (iOS) oder Play Store (Android) suchen → installieren → einloggen.

**Welcher Plan?**

| Plan | Preis | Für wen |
|---|---|---|
| Free | 0 € | Ausprobieren, gelegentliche Nutzung |
| Pro | ~17–20 €/Monat | Tägliche Nutzung, mehr Nachrichten |
| Max | ~100–200 €/Monat | Intensive Nutzung, höchste Limits |
| Team | 20 €/Platz/Monat | 3+ Personen mit geteilten Projekten |

> 💡 Für den Anfang reicht Free. Wenn du merkst, dass du öfter ans Limit stößt — dann auf Pro upgraden.

## Schritt 2 — Claude über dich aufklären

Das ist der wichtigste Schritt. Ohne ihn muss Claude bei jedem Gespräch neu raten, wer du bist und wie du arbeitest. Mit diesem Schritt kennst du Claude ein für allemal.

**Wo du das machst:**
```
Initialen (unten links) anklicken
→ "Profil" öffnen
→ Felder ausfüllen: Name, Beruf, Arbeitsweise
```

Das sind Claudes "Dauererinnerungen" — sie gelten für alle Gespräche auf deinem Account.

**Aber das reicht nicht.** Für wirklich präzise Einstellungen brauchst du den folgenden Onboarding-Prompt. Starte ein neues Gespräch und füge ihn ein — Claude stellt dir dann 50 gezielte Fragen über dich, eine nach der anderen.

### 🎯 Der Onboarding-Prompt (auf Deutsch kopieren)

```
Stell mir genau eine Frage nach der anderen — insgesamt 50 Fragen.
Biete bei jeder Frage wenn möglich 3–5 Antwortoptionen zum Ankreuzen an,
zusätzlich zu einer offenen Antwortmöglichkeit.

Decke dabei diese Bereiche ab:
- Wer ich bin (Beruf, Hintergrund, Erfahrungslevel)
- Was ich tue (Hauptaufgaben, typische Projekte, Ziele)
- Wie ich arbeite (Stil, Tempo, Präferenzen, Tools)
- Was mich an Outputs nervt (Floskeln, Formate, Länge, Ton)
- Wie gute Arbeit für mich aussieht (konkrete Beispiele, Erfolgskriterien)

Wenn meine Antwort vage oder allgemein ist:
Hak nach und bitte um ein konkretes Beispiel oder eine spezifische Situation.

Wenn du alle 50 Fragen gestellt hast:
Fasse alles in einer einzigen, sauber strukturierten .md-Datei zusammen,
die ich direkt in meine Claude-Einstellungen oder Projekt-Anweisungen kopieren kann.
Kein Intro, keine Zusammenfassung — nur die fertige Config-Datei.
```

Wenn Claude fertig ist, bekommst du eine `.md`-Datei mit deinem persönlichen Profil. Dieses kannst du dann:
- In die Profilfelder (Schritt oben) kopieren
- Als Projekt-Anweisung einfügen (→ Schritt 3)
- In jedem neuen Gespräch als Kontext anhängen

## Schritt 3 — Projekte & Custom Instructions einrichten

Projekte sind getrennte Arbeitsbereiche. Jedes Projekt hat seine eigene Erinnerung, eigene Dokumente und eigene Anweisungen.

**Projekt anlegen:**
```
Linke Sidebar → "Neues Projekt" → Name vergeben
→ "Projektanweisungen" schreiben
→ Dokumente hochladen (optional)
```

**Worauf man achten sollte:**

Die Projektanweisungen sollten kurz und präzise sein — maximal eine halbe Seite. Alles, was Claude dauerhaft in diesem Projekt wissen muss: Kontext, Tonfall, Einschränkungen. Aufgaben-spezifische Anweisungen kommen in die jeweilige Nachricht, nicht hierher.

Ein häufiger Fehler: alles in die Projektanweisungen packen, dann ist das Kontext-Budget schnell voll und Claude hat für die eigentliche Aufgabe weniger Platz.

**Was gut in Projektanweisungen gehört:**

```
Ich bin [Rolle] bei [Unternehmen/Kontext].
Zielgruppe meiner Texte: [Beschreibung].
Ton: [formal / locker / technisch].
Immer auf Deutsch antworten.
Keine Floskeln wie "Natürlich!" oder "Ich hoffe, das hilft".
Dokumente in diesem Projekt sind vertraulich — nicht zitieren.
```

**Was NICHT in Projektanweisungen gehört:**
- Die eigentliche Aufgabe ("Schreibe mir einen Blogartikel über...") → gehört in die Nachricht
- Sehr spezifische Formatierungsregeln für Einzelaufgaben → gehört in die Nachricht
- Lange Hintergrundtexte → lieber als Dokument hochladen

**Free-Plan-Limit:** Maximal 5 Projekte. Pro-Plan und höher: unbegrenzt.

**Empfohlene Grundstruktur:**

```
📁 Arbeit        → Firma, Kollegen, Produkte, Tonfall
📁 Privat        → Hobbys, Haushalt, persönliche Ziele
📁 Lernen        → Aktuelles Thema / Kurs / Prüfung
📁 Schreiben     → Blog, Buch, Newsletter — mit Stil-Beispielen
```

> 💡 Lade wichtige Dokumente einmal ins Projekt hoch — Lebenslauf, Styleguide, Verträge. Claude kennt sie dann in jedem Gespräch innerhalb des Projekts, ohne erneutes Hochladen.

**Context-Window in Projekten:**

| Plan | Kontext im Chat |
|---|---|
| Alle bezahlten Pläne | 500K Token mit Opus 4.6+ / Sonnet 4.6 |
| Claude Code | 1M Token (Credits erforderlich) |

> ⚠️ Bekannter Bug: Der RAG-Modus (automatische Dokumentensuche) aktiviert sich manchmal schon ab ca. 13 Dateien, auch wenn noch viel Kontextplatz frei wäre. Bei vielen kleinen Dateien kann Retrieval-Qualität variieren.

← [Zurück zum Alltags-Guide](./README.md)
← [Zurück zur Startseite](../../README.md)
