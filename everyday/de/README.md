<div align="center">

# 🧠 Claude richtig benutzen — Deutsch
### Für alle, die mehr rausholen wollen

</div>

## 📚 Was dich hier erwartet

- [🏠 Teil 1 — Das perfekte Alltags-Setup](#-teil-1--das-perfekte-alltags-setup)
- [🧪 Teil 2 — Geheime Steuerungs-Befehle](#-teil-2--geheime-steuerungs-befehle)
- [✍️ Teil 3 — Menschlich schreiben lassen](#️-teil-3--menschlich-schreiben-lassen)
- [🛡️ Teil 4 — Halluzinationen verhindern](#️-teil-4--halluzinationen-verhindern)
- [⚡ Quick Setup — Startklar in 30 Min.](./quick-setup.md)
- [📋 Vorlagen zum Kopieren](./vorlagen.md)

## 🏠 Teil 1 — Das perfekte Alltags-Setup

### 1.1 Personal Preferences — Claudes Dauererinnerung

Stell dir vor, du hast einen neuen Assistenten. Jeden Tag müsstest du ihm wieder erklären, wer du bist und wie du arbeitest — es sei denn, du gibst ihm einmal eine Karteikarte, die er immer dabei hat.

Genau das sind die **Personal Preferences** in Claude.

So findest du sie:
```
Initialen (unten links) anklicken → "Profil"
→ Felder ausfüllen: Name, Beruf, Arbeitsweise
→ ("What should Claude call you?" / "What best describes your work?")
```

Was du dort einträgst, liest Claude bei jedem einzelnen Gespräch — automatisch, ohne dass du es nochmal erwähnst.

> 💡 Der wichtigste Hinweis dabei: Schreib rein, was Claude **aufhören** soll zu tun — nicht nur was es tun soll. Das macht den größten Unterschied.

#### Was gehört rein?

| Kategorie | Was schreiben |
|---|---|
| 🧑 Wer du bist | Beruf, Hintergrund, Erfahrungslevel |
| 🎯 Dein Ziel | Wofür du Claude hauptsächlich nutzt |
| 📝 Dein Stil | Formal, locker, kurz, ausführlich |
| 🚫 Was nervt | Floskeln, lange Einleitungen, unnötige Aufzählungen |
| 🌍 Sprache | Immer auf Deutsch antworten |

Fertige Vorlagen zum Kopieren gibts hier: [vorlagen.md](./vorlagen.md)

### 1.2 Projekte — Getrennte Arbeitsbereiche für alles

Projekte sind wie separate Schubladen auf deinem Schreibtisch. Jede hat ihre eigene Beschreibung, ihre eigenen Dokumente und ihre eigene Erinnerung.

Wofür Projekte gut sind:
```
📁 Arbeit      → Dokumente, Firmen-Kontext, Kollegennames
📁 Haushalt    → Rezepte, Einkaufslisten, Budget-Tabellen
📁 Hobby       → Projektbeschreibungen, Recherche, Notizen
📁 Lernen      → Lernmaterial, Notizen, Prüfungsvorbereitung
📁 Schreiben   → Stilbeispiele, Charaktere, Handlung
```

So legst du ein Projekt an:
```
claude.ai → linke Sidebar → "Neues Projekt"
→ Namen eingeben → "Projektanweisungen" schreiben
→ Dokumente hochladen (PDF, Word, Excel bis 30 MB)
```

> 💡 Lade wichtige Dokumente einmal ins Projekt hoch — deinen Lebenslauf, einen Styleguide, einen Mietvertrag. Claude kennt sie dann in jedem Gespräch innerhalb des Projekts, ohne dass du sie jedes Mal erneut hochladen musst.

### 1.3 Skills — Wiederverwendbare Mini-Assistenten

Skills (ab Pro-Plan) sind wie Rezepte, die du einmal schreibst und dann immer wieder verwendest.

| Skill-Name | Was er macht |
|---|---|
| 📧 E-Mail-Schreiber | Schreibt E-Mails in deinem persönlichen Stil |
| 🔍 Fakten-Checker | Prüft Quellen, markiert Unsicheres |
| ✏️ Humanizer | Macht KI-Text menschlicher |
| 📊 Meeting-Protokoll | Wandelt Notizen in strukturierte Protokolle um |

So erstellst du einen Skill:
```
claude.ai → Einstellungen → Skills → "Neuer Skill"
→ Name, Beschreibung und Anweisungen eingeben
→ Optional: Beispiel-Dateien hochladen
```

### 1.4 Konnektoren — Claude mit der Außenwelt verbinden

| Konnektor | Was Claude dann kann |
|---|---|
| 🔍 Websuche | Aktuelle Infos aus dem Internet holen |
| 📁 Google Drive | Deine Dokumente lesen und zusammenfassen |
| 📝 Google Docs | Direkt in Dokumente schreiben |
| 📅 Google Calendar | Termine lesen und erstellen |
| 💬 Slack | Nachrichten lesen und schreiben |
| 📓 Notion | Notizen abrufen und aktualisieren |

So aktivierst du Konnektoren:
```
claude.ai → Einstellungen → Konnektoren
→ Dienst auswählen → Verbinden → Erlauben
```

> ⚠️ Claude fragt beim ersten Mal, ob es einen Konnektor verwenden darf. Das ist Absicht — du behältst die Kontrolle.

## 🧪 Teil 2 — Geheime Steuerungs-Befehle

### 2.1 XML-Tags — Claudes geheime Sprache

Das ist der mächtigste Trick überhaupt, und dabei so einfach wie Überschriften in einem Word-Dokument.

XML-Tags sind Wörter in spitzen Klammern, die Claude sagen: *"Hey, dieser Teil hat eine bestimmte Rolle."*

```xml
<rolle>Schreib das hier</rolle>
<aufgabe>Das soll Claude tun</aufgabe>
<kontext>Das ist der Hintergrund</kontext>
```

Warum das funktioniert: Claude wurde auf Millionen von XML-Dokumenten trainiert. Es versteht diese Struktur auf einer tieferen Ebene als normalen Text — ähnlich wie ein Mensch bei einer nummerierten Liste sofort weiß, das sind verschiedene Punkte.

❌ Ohne Tags — Claude rät, was du meinst:
```
Du bist ein Marketingexperte. Analysiere diesen Text: [Produktbeschreibung]
und schreibe danach eine E-Mail-Kampagne dazu.
```

✅ Mit Tags — Claude weiß genau, was was ist:
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
2. Schreibe danach eine 3-teilige E-Mail-Kampagne.
</aufgabe>

<ausgabeformat>
Analyse: 3-5 Bullet Points
E-Mails: Nummeriert, Betreff und Text getrennt
</ausgabeformat>
```

Anthropic-intern gemessen: XML-strukturierte Prompts liefern nachweislich konsistentere Ergebnisse (Anthropic dokumentiert intern bis zu 40% Halluzinationsreduktion).

#### Die 8 wichtigsten Tags

| Tag | Wofür | Beispiel |
|---|---|---|
| `<rolle>` | Wer Claude ist | `<rolle>Steuerfachangestellter, 15 Jahre Erfahrung</rolle>` |
| `<kontext>` | Hintergrundinfos | `<kontext>Ich bin Freiberufler, Steuerjahr 2025</kontext>` |
| `<aufgabe>` | Was Claude tun soll | `<aufgabe>Erkläre, welche Ausgaben ich absetzen kann</aufgabe>` |
| `<anweisungen>` | Regeln und Einschränkungen | `<anweisungen>Nur deutsches Steuerrecht.</anweisungen>` |
| `<dokument>` | Eingefügter Text | `<dokument>Hier der Brief vom Finanzamt...</dokument>` |
| `<beispiele>` | Musterantworten | `<beispiele>So soll die Antwort aussehen: ...</beispiele>` |
| `<denken>` | Claude laut denken lassen | `<denken>Zeige deinen Denkprozess</denken>` |
| `<ausgabeformat>` | Wie die Antwort aussehen soll | `<ausgabeformat>Tabelle: Ausgabe, Betrag, Begründung</ausgabeformat>` |

### 2.2 Tiefes Denken erzwingen

Claude denkt standardmäßig schnell. Für komplexe Aufgaben willst du, dass es langsamer und gründlicher arbeitet.

**Stufe 1 — Der schnelle Trick**

Einfach anhängen:
```
Denke Schritt für Schritt nach, bevor du antwortest.
```

**Stufe 2 — Den Denkweg vorgeben**

```xml
<anweisungen>
Gehe in dieser Reihenfolge vor:
1. Berechne die monatliche Kreditbelastung.
2. Vergleiche Miet- vs. Kaufkosten über 10 und 20 Jahre.
3. Nenne die 3 wichtigsten Argumente für Miete.
4. Nenne die 3 wichtigsten Argumente für Kauf.
5. Gib erst dann deine Empfehlung.
</anweisungen>
```

**Stufe 3 — Denken und Antwort trennen**

```xml
<anweisungen>
Zeige deinen Denkprozess in <denken>-Tags.
Schreibe die finale Antwort in <antwort>-Tags.
Ich lese nur die <antwort>.
</anweisungen>
```

Claude antwortet dann so:

```xml
<denken>
Okay, 380.000 € minus 60.000 € Eigenkapital = 320.000 € Kredit.
Bei 3,5% Zinsen über 25 Jahre wären das ca. 1.600 €/Monat...
</denken>

<antwort>
Monatliche Kreditrate: ca. 1.600 €
Mehrkosten gegenüber Miete: 400 €/Monat
...
</antwort>
```

### 2.3 Antwort-Format kontrollieren

Sag Claude einfach direkt, wie die Antwort aussehen soll:

```xml
<ausgabeformat>
Starte deine Antwort direkt mit: "Die drei wichtigsten Punkte sind:"
Keine Einleitung. Keine Zusammenfassung am Ende.
Nutze Emojis als Aufzählungszeichen (✅, ⚠️, ❌).
Maximal 5 Sätze pro Punkt.
</ausgabeformat>
```

Weitere nützliche Format-Befehle:

| Was du willst | Was du schreibst |
|---|---|
| JSON | `Antworte ausschließlich im JSON-Format.` |
| Tabelle | `Erstelle eine Markdown-Tabelle mit den Spalten: Name, Vor-/Nachteile, Empfehlung.` |
| Kurz | `Maximal 3 Sätze. Keine Aufzählung. Direkte Antwort.` |
| Schritte | `Nummerierte Liste. Jeder Schritt max. 2 Sätze.` |

> Hinweis: Ältere Guides empfehlen manchmal "Prefilling" — den Anfang von Claudes Antwort selbst vorschreiben. Das funktioniert auf aktuellen Modellen nicht mehr und wirft einen Fehler. Die Format-Methode oben ist der aktuelle Ersatz dafür, und ehrlich gesagt der bessere Ansatz.

### 2.4 Rollen-Trick — Claude zum Experten machen

Vage Rolle — kaum Unterschied:
```
Du bist ein Experte. Hilf mir mit Steuern.
```

Präzise Rolle — merklicher Unterschied:
```xml
<rolle>
Du bist Steuerberaterin mit 20 Jahren Erfahrung in Deutschland,
spezialisiert auf Freiberufler im Kreativbereich.
Du erklärst Steuerrecht so, dass auch Nicht-Juristen es sofort verstehen.
Du weist explizit darauf hin, wann ein echter Steuerberater gefragt werden sollte.
</rolle>
```

Warum das so viel ausmacht: Claude hat aus dem Schreiben von Millionen Experten gelernt. Eine präzise Rolle aktiviert die richtigen Muster — Vokabular, Vorsichtsniveau, Erklärtiefe.

### 2.5 Beispiele geben — der unterschätzte Trick

Zeig Claude 3 Beispiele, wie eine Antwort aussehen soll — und die Qualität steigt spürbar.

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

## ✍️ Teil 3 — Menschlich schreiben lassen

### 3.1 Warum KI-Texte so klingen wie KI-Texte

Claude schreibt statistisch. Es wählt immer die Wörter, die am häufigsten vorkommen — das führt zu Texten, die korrekt sind, aber vorhersehbar. Wie ein Schulaufsatz, der alle Anforderungen erfüllt, aber niemanden begeistert.

Das Ergebnis: Alle Claude-Texte klingen ähnlich. KI-Detektoren erkennen das. Menschen auch — sie merken es, ohne genau zu wissen warum.

Die Lösung ist einfacher als man denkt: Du musst Claude sagen, was es **nicht** tun soll. Das ist effektiver als zu sagen, was es tun soll.

### 3.2 Die Verbotsliste — KI-Phrasen raus

Einfach in den Prompt oder die Personal Preferences kopieren:

```
Vermeide folgende Phrasen und Muster komplett:

VERBOTENE FLOSKELN:
- "spielt eine wichtige Rolle"
- "dient als Zeugnis"
- "Es ist wichtig zu beachten, dass"
- "Zusammenfassend lässt sich sagen"
- "In der heutigen schnelllebigen Welt"
- "Natürlich!" / "Gerne!" / "Sicherlich!"
- "Ich hoffe, das hilft"
- "Wenn du weitere Fragen hast"
- "fasziniert weiterhin"
- "unterstreicht die Bedeutung"
- "bahnbrechend" / "revolutionär"

VERBOTENE STRUKTUREN:
- Zwei-Wörter-Überschriften mit Doppelpunkt ("Herausforderungen und Chancen:")
- Aufzählungen am Anfang ohne dass ich darum gebeten habe
- Fettdruck überall ohne echten Grund
- Brief-Eröffnungen wie "Sehr geehrte Damen und Herren"

VERBOTENES FORMAT:
- Gleichlange Absätze
- Immer gleiche Satzlänge
- Immer gleicher Satzanfang
```

### 3.3 Der Humanizer-Prompt

Schreib den Text zuerst mit Claude, dann gib ihn mit diesem Prompt nochmal rein:

```xml
<rolle>
Du bist ein erfahrener Lektor, spezialisiert auf das Eliminieren von KI-Mustern.
Deine Aufgabe ist nicht, den Text besser zu machen — sondern menschlicher.
</rolle>

<aufgabe>
Überarbeite den folgenden Text so, dass er klingt wie von einem echten Menschen geschrieben.
Bewahre die inhaltliche Kernaussage komplett.
</aufgabe>

<anweisungen>
ENTFERNE:
- Alle KI-Standardfloskeln
- Übermäßigen Fettdruck
- Gleichmäßige Absatzlängen
- Immer gleiche Satzstruktur

FÜGE EIN:
- Unterschiedliche Satzlängen (manchmal 3 Wörter, manchmal 30)
- Gelegentliche Gedankensprünge oder Einschübe
- Echte Meinungen statt ausgewogener Neutralität

PRÜFE AM ENDE:
Was macht diesen Text noch offensichtlich KI-generiert?
Beantworte das kurz und korrigiere es dann.
</anweisungen>

<dokument>
[HIER DEINEN TEXT EINFÜGEN]
</dokument>
```

### 3.4 Voice Matching — Dein eigener Stil

Der stärkste Trick überhaupt: Gib Claude ein Beispiel deines eigenen Schreibstils.

```xml
<rolle>
Du bist ein Lektor, der meinen Schreibstil analysiert und nachahmt.
</rolle>

<mein_schreibstil>
[Hier 2-3 Absätze aus deinem eigenen Schreiben einfügen —
eine alte E-Mail, ein Social-Media-Post, was auch immer]
</mein_schreibstil>

<aufgabe>
Analysiere kurz meinen Stil. Schreibe dann diesen Text in meinem Stil um:
</aufgabe>

<text_zum_umschreiben>
[Der KI-Text, den du menschlicher machen willst]
</text_zum_umschreiben>
```

### 3.5 Strukturelle Anti-KI-Anweisungen

Diese Schreibanweisungen produzieren sofort weniger erkennbare KI-Texte:

```
Variiere Satzlängen stark:
- Manchmal 2-4 Wörter. Punkt.
- Manchmal ein langer Satz mit mehreren Einschüben, der sich
  über eine halbe Zeile zieht, weil ein Gedanke eben manchmal Platz braucht.
- Dann wieder kurz.

Variiere Satzanfänge:
- Nicht immer Substantiv + Verb
- Manchmal: "Und dann..."
- Manchmal: "Eigentlich..."
- Manchmal direkt mittendrin anfangen

Keine gleichmäßigen Absätze:
- Manchmal ein Satz.
- Manchmal sieben.
- Kein System dahinter — so wie echte Menschen schreiben.
```

## 🛡️ Teil 4 — Halluzinationen verhindern

### 4.1 Warum Claude manchmal Dinge erfindet

Claude hat keine Suchmaschine. Es erinnert sich an Muster aus dem Training — und wenn es sich nicht sicher ist, füllt es Lücken manchmal mit plausibel klingenden Sachen auf. Das nennt sich Halluzination.

Besonders aufpassen bei:
- 📚 Quellen, Zitate, Bücher, Studien — die werden manchmal erfunden
- 📅 Ereignissen nach Anfang–Mitte 2025 (ältere Modelle) bis Januar 2026 (Opus 4.7) — das ist Claudes Wissens-Cutoff
- 🔢 Genauen Statistiken und Zahlen
- 👤 Wenig bekannte Personen und deren Details

### 4.2 Die Schutzformeln

**Unsicherheit einfordern:**
```
Wenn du dir bei einer Aussage nicht zu 100% sicher bist,
schreibe [NICHT VERIFIZIERT] direkt daneben.
Erfinde keine Quellen. Wenn du eine Quelle nicht kennst: sag das.
```

**Quellen sperren:**
```xml
<anweisungen>
Nenne keine Bücher, Studien, URLs oder Personenzitate,
außer ich habe sie dir in diesem Gespräch gegeben.
Wenn du ein Beispiel brauchst, erstelle ein fiktives und markiere es als [BEISPIEL].
</anweisungen>
```

**Aktualität erzwingen:**
```
Falls diese Information nach Anfang–Mitte 2025 (ältere Modelle) bis Januar 2026 (Opus 4.7) veraltet sein könnte:
Weise mich explizit darauf hin und empfehle, wo ich nachschauen kann.
```

**Selbst-Check einbauen:**
```xml
<anweisungen>
Schreibe deine Antwort.
Prüfe dann selbst: Gibt es Aussagen, bei denen du dir unsicher bist?
Markiere diese mit ⚠️ und erkläre kurz warum.
</anweisungen>
```

### 4.3 Den Fakten-Checker aktivieren

```xml
<rolle>
Du bist ein professioneller Fakten-Checker mit journalistischer Ausbildung.
</rolle>

<aufgabe>
Analysiere den folgenden Text Satz für Satz auf faktische Behauptungen.
</aufgabe>

<anweisungen>
Für jede Behauptung:
✅ WAHRSCHEINLICH KORREKT — du kennst das mit hoher Sicherheit
⚠️ PRÜFEN — du bist unsicher oder es könnte veraltet sein
❌ FALSCH — du weißt, dass es falsch ist

Nenne für jede ⚠️-Markierung, wo man es nachschlagen sollte.
Erfinde selbst keine Fakten, um Lücken zu füllen.
</anweisungen>

<dokument>
[Text einfügen, der geprüft werden soll]
</dokument>
```

### 4.4 Websuche als Sicherheitsnetz

Wenn Claude mit dem Internet verbunden ist:

```
Beantworte diese Frage NUR auf Basis aktueller Suchergebnisse.
Gib für jede Aussage die Quelle an (Webseite + ungefähres Datum).
Falls du keine verlässliche Quelle findest: sag das statt zu raten.
```

Websuche aktivieren: claude.ai → im Chat auf das 🌐-Icon klicken

<div align="center">

## 🎯 Die wichtigsten Regeln auf einen Blick

| Regel | Warum |
|---|---|
| Personal Preferences ausfüllen | Claude kennt dich dann dauerhaft |
| XML-Tags für komplexe Prompts | 20–40% bessere Ergebnisse |
| Verbotsliste für menschlichen Stil | KI-Muster werden eliminiert |
| Quellen nie blind vertrauen | Halluzinations-Schutz |
| Websuche bei aktuellen Themen | Fakten statt Schätzungen |

</div>

→ [Vorlagen & Prompts zum Kopieren](./vorlagen.md)
← [Zurück zur Hauptseite](../README.md)

---

> **⚖️ Rechtliche Hinweise**
>
> **Marken-Disclaimer:** Dieser Guide ist unabhängig erstellt und steht in keiner Verbindung zu Anthropic PBC. Claude™ ist eine Marke von Anthropic. Alle weiteren Marken gehören ihren jeweiligen Inhabern.
>
> **Datenschutz:** Consumer-Nutzer (kostenlos / Pro) sollten beachten, dass Claude-Gespräche standardmäßig zur Modellverbesserung genutzt werden können. Seit der Policy-Änderung im August 2025 ist ein aktiver Opt-out in den Einstellungen erforderlich. „Vertraulich" im Projektkontext bedeutet nicht automatisch, dass Anthropic keinen Zugriff hat — Enterprise-Pläne bieten hier stärkere Garantien.
>
> **Humanizer & KI-Faktencheck:** KI-generierte Faktenchecks ersetzen keine professionelle Prüfung. Der Einsatz von Humanizern im akademischen Kontext kann gegen Hochschul- oder Prüfungsrichtlinien verstoßen — Eigenverantwortung liegt beim Nutzer.
