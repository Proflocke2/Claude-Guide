# Advanced Prompt Engineering für Claude

> Kapitel 1 des Claude Master Guide — Deutsch  
> Themen: XML-Struktur, Chain-of-Thought, Structured Outputs, Role Prompting, Multishot-Beispiele.


## 1. Die XML-Prompt-Architektur

Claude wurde explizit darauf trainiert, XML-Tags als Prompt-Gliederungsprimitive zu erkennen. Es gibt keine kanonischen „Magic Names" — aber Anthropics dokumentierte Konventionen lauten:

| Tag | Position | Zweck |
|---|---|---|
| `<role>` / `<context>` | Zuerst | Wer Claude ist + benötigter Hintergrund |
| `<task>` | Nach dem Kontext | Die Kernaufgabe |
| `<instructions>` | Nach der Aufgabe | Regeln, Einschränkungen, Präferenzen |
| `<document>` / `<documents>` | Oben im Prompt | Quelltexte für Long-Context-Aufgaben |
| `<examples>` / `<example>` | Vor dem Output-Format | Multishot-Muster |
| `<thinking>` | CoT-Scratchpad | Reasoning (getrennt von der Antwort) |
| `<answer>` | Letzter Block | Extrahierbarer Output |
| `<output_format>` | Letzter Block | Genaue Antwortstruktur |

> **Internes Anthropic-Testing:** XML-strukturierte Prompts liefern **20–40% konsistentere Outputs** als unstrukturierte Äquivalente.

### Kanonisches Prompt-Template

```xml
<role>
Du bist ein Senior Data Analyst bei einem Series-B-SaaS-Unternehmen.
Deine Spezialität ist Churn-Analyse und Kohorten-Modellierung.
</role>

<context>
Das Unternehmen hat 12.000 MAU. Churn Rate: 4,2 % MoM.
Stack: Stripe + Mixpanel. Der CFO benötigt eine Board-taugliche Erklärung.
</context>

<task>
Analysiere die beigefügten Kohortendaten und identifiziere die 3 größten Churn-Treiber.
</task>

<instructions>
- Priorisiere statistisch signifikante Treiber (p < 0,05).
- Spekuliere NICHT über die Daten hinaus.
- Verwende Geschäftssprache, keine Statistikfachbegriffe.
- Markiere Datenqualitätsprobleme, falls vorhanden.
</instructions>

<documents>
  <document index="1">
    <source>kohorten_q1_2026.csv</source>
    <document_content>
    [Daten hier einfügen]
    </document_content>
  </document>
</documents>

<output_format>
Gib drei Abschnitte zurück:
1. **Executive Summary** (2–3 Sätze)
2. **Top-3-Churn-Treiber** (Tabelle: Treiber, Evidenz, Konfidenz)
3. **Empfohlene Maßnahmen** (Bullet-Liste, priorisiert nach Impact)
</output_format>
```


## 2. Chain-of-Thought (CoT) — Drei Stufen

CoT nutzen für Aufgaben, die ein Mensch durchdenken müsste: Mehrfaktor-Entscheidungen, mehrstufige Mathematik, komplexe Analysen. Für einfache Lookups überspringen.

### Stufe 1 — Inline-Trigger (Minimal)

Einfach an jeden Prompt anhängen. Kein Overhead.

```xml
<task>
Berechne den Break-Even-Punkt für die folgende SaaS-Preisänderung.
Denke Schritt für Schritt.
</task>
```

### Stufe 2 — Geführte Reasoning-Schritte

Den genauen Denkweg vorgeben, wenn die Domäne klar definiert ist.

```xml
<instructions>
Gehe in folgender Reihenfolge vor:
1. Identifiziere alle Kostenvariablen aus dem Kontext.
2. Identifiziere alle Erlösvariablen.
3. Stelle die Break-Even-Gleichung explizit auf.
4. Löse numerisch.
5. Nenne die getroffenen Annahmen.
Erst dann gib deine Schlussfolgerung an.
</instructions>
```

### Stufe 3 — Strukturierter CoT mit XML-Tags (Programmatische Extraktion)

Optimal für Pipelines, in denen Reasoning und Output separat geloggt, bewertet oder verzweigt werden sollen.

```xml
<instructions>
Verwende <thinking>-Tags für deinen Denkprozess.
Verwende <answer>-Tags nur für die finale Antwort.
Das System extrahiert ausschließlich den <answer>-Inhalt für den Nutzer.
</instructions>
```

Antwortstruktur, die programmatisch geparst werden kann:

```xml
<thinking>
Der Nutzer fragt nach X. Zunächst muss ich bedenken...
Einschränkung A schließt Option 2 aus, weil...
Daher ist der richtige Ansatz...
</thinking>

<answer>
[Saubere, nutzerorientierte Antwort hier]
</answer>
```

> **Extended-Thinking-Modelle:** Für `claude-opus-4-8` mit `effort: max` allgemeine Anweisungen bevorzugen statt prescriptiver Schritte. Anthropic: *„Claudes Kreativität übersteigt möglicherweise deine Fähigkeit, den optimalen Denkprozess vorzuschreiben."*


## 3. Structured Outputs — Prefilling ersetzen

**Prefilling ist deprecated.** Das Senden von `{` als Beginn eines Assistant-Turns (für JSON-Forcing) gibt auf aktuellen Modellen einen 400-Fehler zurück.

### Option A — `output_config.format` (Garantiertes JSON via Schema)

```python
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    output_config={
        "format": {
            "type": "json_schema",
            "json_schema": {
                "name": "churn_analyse",
                "schema": {
                    "type": "object",
                    "properties": {
                        "treiber": {
                            "type": "array",
                            "items": {
                                "type": "object",
                                "properties": {
                                    "name": {"type": "string"},
                                    "konfidenz": {"type": "number"},
                                    "evidenz": {"type": "string"}
                                },
                                "required": ["name", "konfidenz", "evidenz"]
                            }
                        },
                        "executive_summary": {"type": "string"},
                        "datenqualitaets_flags": {
                            "type": "array",
                            "items": {"type": "string"}
                        }
                    },
                    "required": ["treiber", "executive_summary"]
                },
                "strict": True
            }
        }
    },
    messages=[{"role": "user", "content": "Analysiere diese Churn-Daten: [...]"}]
)
```

### Option B — `strict: true` auf Tool-Definitionen

Das **Structured-Output-as-Tool**-Pattern verwenden, wenn garantierte Schema-Eingaben benötigt werden:

```python
tools = [
    {
        "name": "ergebnis_zurueckgeben",
        "description": "Gibt das strukturierte Analyseergebnis zurück. Genau einmal aufrufen.",
        "input_schema": {
            "type": "object",
            "properties": {
                "treiber": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "name": {"type": "string"},
                            "konfidenz": {"type": "number", "minimum": 0, "maximum": 1},
                            "evidenz": {"type": "string"}
                        },
                        "required": ["name", "konfidenz", "evidenz"]
                    }
                },
                "zusammenfassung": {"type": "string"}
            },
            "required": ["treiber", "zusammenfassung"]
        },
        "strict": True
    }
]

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    tools=tools,
    tool_choice={"type": "tool", "name": "ergebnis_zurueckgeben"},
    messages=[{"role": "user", "content": "Analysiere diese Daten: [...]"}]
)

tool_use = next(b for b in response.content if b.type == "tool_use")
ergebnis = tool_use.input  # garantiert schema-konform
```


## 4. Role Prompting

Rolle immer im **`system`-Parameter** platzieren, nicht in der User-Message.

```python
system_prompt = """
<role>
Du bist General Counsel eines Fortune-500-Technologieunternehmens.
Du verfügst über 20 Jahre Erfahrung in SaaS-Lizenzierung, DSGVO-Compliance und M&A Due Diligence.
Du kommunizierst in präziser Rechtssprache, zeigst aber stets das Geschäftsrisiko klar auf.
</role>

<context>
Du berätst das Produktteam bei Fragen zu API-Nutzungsbedingungen.
Du gibst keine abschließenden Rechtsauskünfte — du rahmst Risiken und empfiehlst Eskalationsschwellen.
</context>
"""
```


## 5. Multishot-Beispiele

**3–5 diverse, relevante, strukturierte Beispiele** ist der dokumentierte Sweet Spot. In `<examples>`-Tags einwickeln.

```xml
<examples>
  <example>
    <input>Kunde churnte nach 45 Tagen. Letzter Login: 3 Tage vor Kündigung. Support-Tickets: 0.</input>
    <output>
    Klassifikation: Silent Churn
    Primärtreiber: Aktivierungsfehler (niedriges Engagement-Muster)
    Konfidenz: Hoch
    Empfohlene Maßnahme: Retroaktives Onboarding-Audit für ähnliche Kohorte
    </output>
  </example>

  <example>
    <input>Kunde churnte nach 14 Monaten. 3 Support-Tickets in letzten 30 Tagen. Themen: Abrechnungsstreit, Feature-Request 2× abgelehnt.</input>
    <output>
    Klassifikation: Unzufriedenheits-Churn
    Primärtreiber: Ungelöste Reibung (Support-Eskalationsmuster)
    Konfidenz: Mittel-Hoch
    Empfohlene Maßnahme: Win-back-Sequenz mit Feature-Roadmap-Kontext
    </output>
  </example>
</examples>
```


## 6. Long-Context-Dokumentenhandling

```xml
<documents>
  <document index="1">
    <source>vertrag_2024.pdf</source>
    <document_content>[vollständiger Text]</document_content>
  </document>
  <document index="2">
    <source>vertrag_2026.pdf</source>
    <document_content>[vollständiger Text]</document_content>
  </document>
</documents>

<task>
Vergleiche die Haftungsklauseln zwischen Dokument 1 und Dokument 2.
Zitiere zuerst die relevanten Abschnitte in <quotes>-Tags.
Liefere dann deine Analyse.
</task>
```

> **Layout-Regel:** Dokumente **oben** im Prompt, Query und Instructions **unten**. Anthropic berichtet bis zu **30% Qualitätsverbesserung** bei langen Kontexten durch diese Anordnung.


*← [Zurück zur deutschen README](./README.md) | Weiter: [Modell-Matrix →](./model-matrix.md)*
