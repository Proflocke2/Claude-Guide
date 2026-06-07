<div align="center">

# 🧠 Der ultimative Claude-Guide — Deutsch
### *Für alle, die mehr aus Claude herausholen wollen*

</div>

---

## 📚 Inhaltsverzeichnis

- [🏠 Teil 1 — Das perfekte Alltags-Setup](#-teil-1--das-perfekte-alltags-setup)
- [🧪 Teil 2 — Geheime Steuerungs-Befehle](#-teil-2--geheime-steuerungs-befehle)
- [✍️ Teil 3 — Menschlich schreiben lassen](#️-teil-3--menschlich-schreiben-lassen)
- [🛡️ Teil 4 — Halluzinationen verhindern](#️-teil-4--halluzinationen-verhindern)
- [📋 Vorlagen](./vorlagen.md)

---

## 🏠 Teil 1 — Das perfekte Alltags-Setup

### 1.1 Personal Preferences — Claudes Dauererinnerung

Stelle dir vor, du hast einen neuen Assistenten. Jeden Tag musst du ihm wieder erklären, wer du bist und wie du arbeitest — es sei denn, du schreibst ihm einmal eine Karteikarte, die er immer dabei hat.

Genau das sind die **Personal Preferences** (Persönliche Einstellungen) in Claude.

**So findest du sie:**
```
claude.ai → unten links auf dein Profil-Icon klicken
→ "Einstellungen" → "Profil"
→ Feld: "Welche Präferenzen soll Claude in Antworten berücksichtigen?"
```

Das, was du dort einträgst, liest Claude **bei jedem einzelnen Gespräch** — automatisch, ohne dass du es noch einmal erwähnst.

> 💡 **Die wichtigste Erkenntnis:** Schreib dort, was Claude **aufhören** soll zu tun — nicht nur was es tun soll. Das macht den größten Unterschied.

#### ✅ Was gehört rein?

| Kategorie | Was schreiben |
|---|---|
| 🧑 Wer du bist | Beruf, Hintergrund, Erfahrungslevel |
| 🎯 Dein Ziel | Was du meistens von Claude willst |
| 📝 Dein Stil | Formal, locker, kurz, ausführlich |
| 🚫 Was du hasst | Floskeln, lange Einleitungen, Aufzählungen |
| 🌍 Sprache | Immer auf Deutsch antworten |

→ **Fertige Vorlagen zum Kopieren:** [vorlagen.md](./vorlagen.md)

---

### 1.2 Projekte — Getrennte Arbeitsbereiche für alles

Projekte sind wie **separate Schubladen** auf deinem Schreibtisch. Jede Schublade hat ihre eigene Beschreibung, ihre eigenen Dokumente und ihre eigene Erinnerung.

**Wofür sind Projekte gut?**

```
📁 Arbeit          → Dokumente, Firmen-Kontext, Kollegen-Namen
📁 Haushalt        → Rezepte, Einkaufslisten, Budget-Tabellen  
📁 Hobby           → Projektbeschreibungen, Recherche, Notizen
📁 Lernen          → Lernmaterial, Notizen, Prüfungsvorbereitung
📁 Schreiben       → Stil-Beispiele, Charaktere, Handlung
```

**So legst du ein Projekt an:**
```
claude.ai → linke Sidebar → "Neues Projekt"
→ Namen eingeben → "Projektanweisungen" schreiben
→ Dokumente hochladen (PDF, Word, Excel bis 30 MB)
```

**Das Geheimnis der Projektanweisungen:**  
Schreib kurz und präzise rein, was Claude in DIESEM Projekt immer wissen soll. Aufgaben-spezifische Anweisungen kommen dann in die eigentliche Nachricht.

> 💡 **Profi-Tipp:** Lade wichtige Dokumente einmal ins Projekt hoch (z. B. dein Lebenslauf, dein Firmen-Styleguide, ein Mietvertrag) — Claude kennt sie dann in jedem Gespräch innerhalb des Projekts, ohne dass du sie jedes Mal erneut hochladen musst.

---

### 1.3 Skills — Claudes wiederverwendbare Mini-Assistenten

Skills (verfügbar ab Pro-Plan) sind wie **Rezepte**, die du einmal schreibst und dann immer wieder verwenden kannst.

**Beispiele für nützliche Skills:**

| Skill-Name | Was er macht |
|---|---|
| 📧 E-Mail-Schreiber | Schreibt E-Mails in deinem persönlichen Stil |
| 🔍 Fakten-Checker | Prüft Quellen und markiert Unsicheres |
| ✏️ Humanizer | Macht KI-Text menschlicher (mehr dazu in Teil 3) |
| 📊 Meeting-Protokoll | Wandelt Notizen in strukturierte Protokolle um |

**So erstellst du einen Skill:**
```
claude.ai → Einstellungen → Skills → "Neuer Skill"
→ Name, Beschreibung und Anweisungen eingeben
→ Optional: Beispiel-Dateien hochladen
```

---

### 1.4 Konnektoren — Claude mit der Außenwelt verbinden

Claude kann sich mit anderen Apps verbinden und dort Informationen abrufen oder Aktionen ausführen.

**Verfügbare Konnektoren (Auswahl):**

| Konnektor | Was Claude dann kann |
|---|---|
| 🔍 Websuche | Aktuelle Informationen aus dem Internet abrufen |
| 📁 Google Drive | Deine Dokumente lesen und zusammenfassen |
| 📝 Google Docs | Direkt in Dokumente schreiben |
| 📅 Google Calendar | Termine lesen und erstellen |
| 💬 Slack | Nachrichten lesen und schreiben |
| 📓 Notion | Notizen abrufen und aktualisieren |

**So aktivierst du Konnektoren:**
```
claude.ai → Einstellungen → Konnektoren (oder "Verbindungen")
→ Gewünschten Dienst auswählen → Verbinden → Erlauben
```

> ⚠️ **Wichtig:** Claude fragt dich beim ersten Mal, ob es einen Konnektor verwenden darf. Das ist Absicht — du behältst die Kontrolle.

---

## 🧪 Teil 2 — Geheime Steuerungs-Befehle

### 2.1 XML-Tags — Claudes geheime Sprache

Das ist der **mächtigste Trick** überhaupt, und er ist so einfach wie ein Word-Dokument mit Überschriften.

**Was sind XML-Tags?**  
Es sind Wörter in spitzen Klammern, die Claude sagen: *„Hey, dieser Teil hat eine bestimmte Rolle."*

```xml
<rolle>Schreib das hier</rolle>
<aufgabe>Das soll Claude tun</aufgabe>
<kontext>Das ist der Hintergrund</kontext>
```

**Warum funktioniert das?**  
Claude wurde auf Millionen von XML-Dokumenten trainiert. Es "versteht" diese Struktur auf einer tieferen Ebene als normalen Text — ähnlich wie ein Mensch bei einer nummerierten Liste sofort weiß: das sind verschiedene Punkte.

**Vergleich: Mit und ohne XML-Tags**

❌ **Ohne Tags** — Claude rät, was du meinst:
```
Du bist ein Marketingexperte. Analysiere diesen Text: [Produktbeschreibung] 
und schreibe danach eine E-Mail-Kampagne dazu.
```

✅ **Mit Tags** — Claude weiß genau, was was ist:
```xml
<rolle>Du bist ein erfahrener Marketing-Experte für B2B-Software.</rolle>

<kontext>
Wir launchen ein neues Projektmanagement-Tool für kleine Teams.
Zielgruppe: Selbstständige und kleine Agenturen.
</kontext>

<dokument>
[Hier die Produktbeschreibung einfügen]
</dokument>

<aufgabe>
1. Analysiere die Stärken und Schwächen der Produktbeschreibung.
2. Schreibe danach eine 3-teilige E-Mail-Kampagne (Betreff + Text jeweils).
</aufgabe>

<ausgabeformat>
Analyse: 3-5 Bullet Points
E-Mails: Nummeriert, mit "Betreff:" und "Text:" getrennt
</ausgabeformat>
```

**Anthropic-Daten:** Strukturierte Prompts mit XML-Tags liefern 20–40% konsistentere Ergebnisse als unstrukturierter Text.

#### 🏷️ Die 8 wichtigsten Tags für den Alltag

| Tag | Wofür | Beispiel |
|---|---|---|
| `<rolle>` | Wer Claude ist | `<rolle>Steuerfachangestellter mit 15 Jahren Erfahrung</rolle>` |
| `<kontext>` | Hintergrundinformationen | `<kontext>Ich bin Freiberufler, Steuerjahr 2025</kontext>` |
| `<aufgabe>` | Was Claude tun soll | `<aufgabe>Erkläre mir, welche Ausgaben ich absetzen kann</aufgabe>` |
| `<anweisungen>` | Regeln und Einschränkungen | `<anweisungen>Nur deutsche Steuergesetze. Keine Spekulation.</anweisungen>` |
| `<dokument>` | Eingefügter Text/Inhalt | `<dokument>Hier der Brief vom Finanzamt...</dokument>` |
| `<beispiele>` | Muster-Antworten | `<beispiele>So soll die Antwort aussehen: ...</beispiele>` |
| `<denken>` | Claude laut denken lassen | `<denken>Zeige mir deinen Denkprozess</denken>` |
| `<ausgabeformat>` | Wie die Antwort aussehen soll | `<ausgabeformat>Tabelle mit 3 Spalten: Ausgabe, Betrag, Begründung</ausgabeformat>` |

---

### 2.2 Tiefes Denken erzwingen

Claude denkt standardmäßig schnell. Für komplexe Aufgaben willst du, dass es langsamer und gründlicher arbeitet. Diese drei Techniken funktionieren sofort.

#### Stufe 1 — Der schnelle Trick (1 Satz)
Hänge einfach an jeden komplexen Prompt dran:
```
Denke Schritt für Schritt nach, bevor du antwortest.
```

#### Stufe 2 — Den Denkweg vorgeben
```xml
<aufgabe>
Ich überlege, ob ich meine Wohnung kaufen oder weiter mieten soll.
Monatliche Miete: 1.200 €. Kaufpreis: 380.000 €. Eigenkapital: 60.000 €.
</aufgabe>

<anweisungen>
Analysiere in dieser Reihenfolge:
1. Berechne die monatliche Kreditbelastung beim Kauf (Zinssatz ca. 3,5%).
2. Vergleiche Miet- vs. Kaufkosten über 10 und 20 Jahre.
3. Nenne die 3 wichtigsten Faktoren, die für Miete sprechen.
4. Nenne die 3 wichtigsten Faktoren, die für Kauf sprechen.
5. Gib dann deine Empfehlung.
Erst nach Schritt 5 kommt deine Schlussfolgerung.
</anweisungen>
```

#### Stufe 3 — Denken und Antwort trennen (für maximale Klarheit)
```xml
<anweisungen>
Zeige deinen Denkprozess in <denken>-Tags.
Schreibe die finale Antwort für mich in <antwort>-Tags.
Ich lese nur die <antwort>.
</anweisungen>
```

Claude antwortet dann so:
```xml
<denken>
Okay, ich muss zuerst bedenken... Der Kaufpreis von 380.000 € minus
60.000 € Eigenkapital ergibt 320.000 € Kredit. Bei 3,5% Zinsen und
25 Jahren Laufzeit wären das ca. 1.600 €/Monat...
</denken>

<antwort>
Basierend auf deinen Zahlen:
**Monatliche Kreditrate:** ca. 1.600 €
**Mehrkosten vs. Miete:** 400 €/Monat
...
</antwort>
```

> 💡 **Wann lohnt sich Stufe 3?** Immer, wenn du die Antwort direkt kopieren oder jemandem zeigen möchtest — ohne Claudes Überlegungen darin.

---

### 2.3 Antwort-Format kontrollieren (der frühere "Prefilling"-Trick)

Früher konnte man Claude den Anfang seiner eigenen Antwort vorgeben, um das Format zu erzwingen. Dieser Trick funktioniert auf aktuellen Claude-Modellen nicht mehr.

**Was funktioniert stattdessen — und sogar besser:**

Erkläre das gewünschte Format direkt im `<ausgabeformat>`-Tag:

```xml
<ausgabeformat>
Starte deine Antwort direkt mit: "Die drei wichtigsten Punkte sind:"
Keine Einleitung. Keine Zusammenfassung am Ende.
Nutze Emojis als Aufzählungszeichen (✅, ⚠️, ❌).
Maximal 5 Sätze pro Punkt.
</ausgabeformat>
```

**Weitere nützliche Format-Befehle:**

| Was du willst | Was du schreibst |
|---|---|
| JSON-Format | `Antworte ausschließlich im JSON-Format. Kein Text davor oder danach.` |
| Tabelle | `Erstelle eine Markdown-Tabelle mit den Spalten: Name, Vor- und Nachteile, Empfehlung.` |
| Kurz und knapp | `Maximal 3 Sätze. Keine Aufzählung. Direkte Antwort.` |
| Schritt-für-Schritt | `Nummerierte Liste. Jeder Schritt: max. 2 Sätze. Fange mit dem ersten Schritt an.` |
| E-Mail-Format | `Schreibe eine fertige E-Mail. Betreff: [...]. Kein Platzhalter, keine eckigen Klammern.` |

---

### 2.4 Rollen-Trick — Claude zu einem Experten machen

Das Zuweisen einer konkreten Rolle ist einer der wirkungsvollsten Tricks überhaupt.

**Vage Rolle** — kaum Unterschied:
```
Du bist ein Experte. Hilf mir mit Steuern.
```

**Präzise Rolle** — merklicher Unterschied:
```xml
<rolle>
Du bist Steuerberaterin mit 20 Jahren Erfahrung in Deutschland,
spezialisiert auf Freiberufler und Selbstständige im Kreativbereich.
Du erklärst Steuerrecht so, dass auch Nicht-Juristen es sofort verstehen.
Du weist explizit darauf hin, wenn etwas ins Steuerberatungs-Territorium fällt
und dann ein Profi gefragt werden sollte.
</rolle>
```

**Warum funktioniert das?** Claude lernt aus dem Schreiben von Millionen von Experten. Eine präzise Rolle aktiviert die richtigen Muster — Vokabular, Vorsichtsniveau, Erklärtiefe.

---

### 2.5 Beispiele geben — der unterschätzte Superpowertrick

Zeig Claude **3 Beispiele**, wie eine Antwort aussehen soll — und die Qualität steigt messbar.

```xml
<beispiele>
  <beispiel>
    <eingabe>Produkt: Kaffeebecher aus Bambus</eingabe>
    <ausgabe>Dein Morgenkaffee, neu gedacht. Nachhaltig, leicht, spülmaschinenfest — der Becher, der mit dir in Rente geht.</ausgabe>
  </beispiel>

  <beispiel>
    <eingabe>Produkt: Laufschuhe für Frauen</eingabe>
    <ausgabe>Gebaut für deinen Rhythmus. Nicht für Fotos. Für Kilometer.</ausgabe>
  </beispiel>

  <beispiel>
    <eingabe>Produkt: Stehschreibtisch</eingabe>
    <ausgabe>Acht Stunden sitzen war gestern. Dein Rücken hat abgestimmt.</ausgabe>
  </beispiel>
</beispiele>

<aufgabe>Schreibe jetzt einen Slogan für: Noise-Cancelling-Kopfhörer für Pendler</aufgabe>
```

---

## ✍️ Teil 3 — Menschlich schreiben lassen

### 3.1 Warum KI-Texte so klingen wie KI-Texte

Claude schreibt statistisch. Es wählt immer die Wörter, die am häufigsten vorkommen — das führt zu Texten, die korrekt, aber vorhersehbar sind. Wie ein Schulaufsatz, der alle Anforderungen erfüllt, aber niemanden begeistert.

**Das Ergebnis:** Alle Claude-Texte klingen ähnlich. KI-Detektoren erkennen das. Menschen auch — sie merken es, ohne zu wissen warum.

**Die Lösung:** Du musst Claude sagen, was es NICHT tun soll. Das ist effektiver als zu sagen, was es tun soll.

---

### 3.2 Die Verbotsliste — KI-Phrasen eliminieren

Füge diese Liste direkt in deinen Prompt oder in deine Personal Preferences ein:

```
Vermeide folgende Phrasen und Muster komplett:

VERBOTENE FLOSKELN:
- "spielt eine wichtige Rolle"
- "dient als Zeugnis" / "serves as a testament"
- "Es ist wichtig zu beachten, dass"
- "Zusammenfassend lässt sich sagen"
- "In der heutigen schnelllebigen Welt"
- "Natürlich!" / "Gerne!" / "Sicherlich!"
- "Ich hoffe, das hilft"
- "Wenn du weitere Fragen hast"
- "fasziniert weiterhin"
- "unterstreicht die Bedeutung"
- "reiches kulturelles Erbe"
- "bahnbrechend" / "revolutionär"
- "essentiell" / "facettenreich"

VERBOTENE STRUKTUREN:
- Zwei-Wörter-Überschriften mit Doppelpunkt ("Herausforderungen und Chancen:")
- Aufzählung am Anfang, ohne dass ich sie verlangt habe
- Überall Fettdruck ohne echten Grund
- Trennlinien (---) zwischen jedem Abschnitt
- Brief-Eröffnungen wie "Sehr geehrte Damen und Herren"

VERBOTENES FORMAT:
- Gleichlange Absätze
- Immer gleiche Satzlänge
- Immer gleicher Satzanfang (Subject + Verb)
```

---

### 3.3 Der Humanizer-Prompt — der Alleskönner

Schreibe zuerst den Text mit Claude, dann gib ihn mit diesem Prompt nochmal ein:

```xml
<rolle>
Du bist ein erfahrener Lektor, der auf das Eliminieren von KI-Mustern 
in Texten spezialisiert ist. Deine Aufgabe ist nicht, den Text besser zu machen —
sondern ihn menschlicher zu machen.
</rolle>

<aufgabe>
Überarbeite den folgenden Text so, dass er klingt wie von einem echten Menschen 
geschrieben — mit natürlichen Unvollkommenheiten, variierenden Satzlängen und 
echter Persönlichkeit. Bewahre die inhaltliche Kernaussage komplett.
</aufgabe>

<anweisungen>
ENTFERNE:
- Alle KI-Standardfloskeln (siehe Liste oben)
- Übermäßigen Fettdruck
- Künstliche Zweigliedrige Überschriften
- Gleichmäßige Absatzlängen
- Immer gleiche Satzstruktur

FÜGE EIN:
- Unterschiedliche Satzlängen (manchmal 3 Wörter, manchmal 30)
- Einen gelegentlichen Gedankensprung oder Einschub
- Natürlichen Ton, der zur Zielgruppe passt
- Echte Meinungen statt ausgewogener Neutralität

PRÜFE AM ENDE:
Frage dich: "Was macht diesen Text noch offensichtlich KI-generiert?" 
Beantworte das kurz und korrigiere es dann.
</anweisungen>

<dokument>
[HIER DEINEN TEXT EINFÜGEN]
</dokument>
```

---

### 3.4 Claudes eigene Stimme — Voice Matching

Der stärkste Humanizer-Trick: Gib Claude ein Beispiel deines eigenen Schreibstils.

```xml
<rolle>
Du bist ein Lektor, der meinen Schreibstil analysiert und nachahmt.
</rolle>

<mein_schreibstil>
[Hier 2-3 Absätze aus deinem eigenen Schreiben einfügen — eine alte E-Mail, 
ein Social-Media-Post, ein Brief, whatever du schon geschrieben hast]
</mein_schreibstil>

<aufgabe>
Analysiere zuerst kurz meinen Stil (Satzlänge, Tonfall, Besonderheiten).
Schreibe dann diesen Text in meinem Stil um:
</aufgabe>

<text_zum_umschreiben>
[Der KI-Text, den du menschlicher machen willst]
</text_zum_umschreiben>
```

**Warum das funktioniert:** Claude lernt deine Rhythmen — kurze Sätze, Lieblingsworte, ob du Doppelpunkte liebst oder Gedankenstriche — und überträgt sie auf den neuen Text.

---

### 3.5 Strukturelle Anti-KI-Muster

Diese Schreibanweisungen produzieren sofort weniger erkennbare KI-Texte:

```
SATZLÄNGEN variieren:
- Manchmal 2-4 Wörter. Punkt.
- Manchmal ein langer Satz mit mehreren Einschüben, der sich über eine halbe Zeile zieht, 
  weil ein Gedanke eben manchmal Zeit braucht.
- Dann wieder kurz.

SATZANFÄNGE variieren:
- Nicht immer: "Substantiv + Verb"
- Manchmal: "Und dann..."
- Manchmal: "Eigentlich..."
- Manchmal mit einer Frage beginnen.
- Manchmal direkt mitten im Gedanken.

KEINE GLEICHMÄSSIGEN ABSÄTZE:
- Manchmal ein Satz.
- Manchmal sieben.
- Kein System dahinter — so wie echte Menschen schreiben.
```

---

## 🛡️ Teil 4 — Halluzinationen verhindern

### 4.1 Warum Claude manchmal Dinge erfindet

Claude hat keine Suchmaschine. Es "erinnert" sich an Muster aus dem Training — und wenn es sich nicht sicher ist, füllt es Lücken manchmal mit plausibel klingenden Dingen auf. Das nennt sich Halluzination.

**Besonders gefährlich bei:**
- 📚 Quellen, Zitate, Bücher, Studien (werden erfunden!)
- 📅 Aktuellen Ereignissen nach August 2025 (Wissens-Cutoff)
- 🔢 Genauen Statistiken und Zahlen
- 👤 Biographischen Details über wenig bekannte Personen

---

### 4.2 Die Halluzinations-Schutzformeln

**Formel 1 — Unsicherheit einfordern:**
```
Wenn du dir bei einer Aussage nicht zu 100 % sicher bist, 
schreibe "[NICHT VERIFIZIERT]" direkt daneben.
Erfinde keine Quellen. Wenn du eine Quelle nicht kennst: sag das.
```

**Formel 2 — Quellen explizit sperren:**
```xml
<anweisungen>
Nenne KEINE Bücher, Studien, URLs oder Personen-Zitate, 
außer ich habe sie dir in diesem Gespräch gegeben.
Wenn du ein Beispiel brauchst, erstelle ein fiktives und markiere es als "[BEISPIEL]".
</anweisungen>
```

**Formel 3 — Aktualität erzwingen:**
```
Falls diese Information nach August 2025 veraltet sein könnte:
Weise mich explizit darauf hin und empfehle, wo ich nachschauen soll.
```

**Formel 4 — Selbst-Überprüfung einbauen:**
```xml
<anweisungen>
Schreibe deine Antwort.
Prüfe dann selbst: Gibt es Aussagen, bei denen du dir unsicher bist?
Markiere diese mit ⚠️ und erkläre kurz warum.
</anweisungen>
```

---

### 4.3 Den Fakten-Checker aktivieren

Wenn du einen Text auf Fakten prüfen willst:

```xml
<rolle>
Du bist ein professioneller Fakten-Checker mit journalistischer Ausbildung.
</rolle>

<aufgabe>
Analysiere den folgenden Text Satz für Satz auf faktische Behauptungen.
</aufgabe>

<anweisungen>
Für jede Behauptung:
- ✅ WAHRSCHEINLICH KORREKT: Wenn du das aus deinem Training mit hoher Sicherheit kennst
- ⚠️ PRÜFEN: Wenn du es nicht sicher weißt oder es veraltet sein könnte
- ❌ FALSCH: Wenn du weißt, dass es falsch ist

Nenne für jede "PRÜFEN"-Markierung, wo man es nachschlagen sollte.
Erfinde selbst keine Fakten, um Lücken zu füllen.
</anweisungen>

<dokument>
[Hier den Text einfügen, der geprüft werden soll]
</dokument>
```

---

### 4.4 Websuche als Sicherheitsnetz

Wenn Claude mit dem Internet verbunden ist (Websuche aktiviert), kannst du Halluzinationen fast komplett ausschließen:

```
Beantworte diese Frage NUR auf Basis aktueller Suchergebnisse.
Gib für jede Aussage die Quelle an (Webseite + ungefähres Datum).
Falls du keine verlässliche Quelle findest: sag das statt zu raten.
```

> ⚠️ **Websuche aktivieren:** claude.ai → im Chat auf das 🌐-Icon klicken

---

<div align="center">

## 🎯 Auf einen Blick — Die wichtigsten Regeln

| # | Regel | Warum |
|---|---|---|
| 1 | Personal Preferences ausfüllen | Claude kennt dich dann dauerhaft |
| 2 | XML-Tags für komplexe Prompts | 20–40% bessere Ergebnisse |
| 3 | Verbotslist für menschlichen Stil | KI-Muster werden eliminiert |
| 4 | Niemals Quellen ohne Gegenchecks | Halluzinations-Schutz |
| 5 | Websuche aktivieren bei aktuellen Themen | Fakten statt Schätzungen |

</div>

---

*→ [Vorlagen & Prompts zum Kopieren](./vorlagen.md)*  
*← [Zurück zur Hauptseite](../README.md)*
