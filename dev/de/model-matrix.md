# Modell-Auswahlmatrix

> Kapitel 2 des Claude Master Guide — Deutsch  
> Stand: Juni 2026. Veraltete IDs werden am 15. Juni 2026 deaktiviert.


## Aktuelle Produktionsmodelle

| Modell-ID | Kontext | Max. Output | Batch-Output | Preis (in / out) |
|---|---|---|---|---|
| `claude-opus-4-8` | 1M Token | 128K | 300K (Beta-Header) | $5 / $25 pro MTok |
| `claude-sonnet-4-6` | 1M Token | 64K | 300K (Beta-Header) | $3 / $15 pro MTok |
| `claude-haiku-4-5` | 200K Token | 64K | — | $1 / $5 pro MTok |

Batch API: Pauschal 50% Rabatt auf alle Aufrufe; verlängert Max-Output auf 300K bei Opus 4.6+/Sonnet 4.6 via `output-300k-2026-03-24` Beta-Header. Kein Qualitätsunterschied für nicht-echtzeitkritische Workloads.

**Prompt-Caching-Multiplikatoren:** 5-Min-Schreibvorgänge 1,25×, 1-Stunden-Schreibvorgänge 2×, Cache-Lesevorgänge 0,1× des Basis-Input-Preises. Caching amortisiert sich ab einem Lesevorgang (5-min TTL) oder zwei Lesevorgängen (1-Stunden-TTL).


## Entscheidungsflussdiagramm

```
Start: Was erfordert deine Aufgabe?
│
├─ Autonome Agenten (>10 Min.), komplexe Multi-File-Refactors,
│  juristische/finanzielle Analyse, Long-Horizon-Orchestrierung
│  └─→  claude-opus-4-8
│       (Einziges Modell, das Anthropics internen "Super-Agent"-Benchmark besteht;
│        84% auf OnlineMind2Web für Computer Use)
│
├─ Produktions-Chat, mittleres Coding, RAG QA, Dokumentenanalyse,
│  Frontend-Design, Computer Use, die meisten agentischen Workloads
│  └─→  claude-sonnet-4-6  ← STANDARD
│       (Nahezu Opus-Qualität bei 60% geringeren Kosten; stärkeres
│        Instruction-Following als 4.5; Standard für den Großteil des Produktionsverkehrs)
│
└─ Klassifikation, Extraktion, Zusammenfassung, Routing,
   Sub-Agent-Fan-out, hochvolumige parallele Ausführung, Free-Tier-Produkte
   └─→  claude-haiku-4-5
        (Entspricht Sonnet 4 bei Reasoning/Coding bei einem Bruchteil der Latenz;
         erstes Haiku mit Extended Thinking, Computer Use, Bash, Websuche;
         73,3% SWE-bench Verified)
```


## Fähigkeitsvergleich

| Fähigkeit | Opus 4.8 | Sonnet 4.6 | Haiku 4.5 |
|---|:---:|:---:|:---:|
| Frontier-Reasoning | ✅ Beste | ✅ Stark | ✅ Gut |
| Agentisches Coding | ✅ Beste | ✅ Stark | ⚠️ Nur Sub-Agent |
| Computer Use (OSWorld-Verified) | 84% | 61,4% | ✅ Unterstützt |
| Extended Thinking | ✅ | ✅ | ✅ (erstes Haiku) |
| Parallele Tool-Aufrufe | ✅ | ✅ | ✅ |
| Structured Outputs | ✅ | ✅ | ✅ |
| Strict Tool Schemas | ✅ | ✅ | ✅ |
| Batch API (300K Output) | ✅ | ✅ | ❌ |
| 1M-Kontextfenster | ✅ | ✅ | ❌ (200K) |
| `effort: max` | ✅ | ❌ | ❌ |


## Routing-Strategie nach Use Case

### Produktions-API-Traffic

```python
def modell_routing(aufgabe: dict) -> str:
    """
    Regelbasierter Modell-Router zur Kostenoptimierung.
    CloudZero-Kundendaten: Einfaches Routing senkt gemischte Kosten um 40–60%.
    """
    # Tier 1 — Haiku (Klassifikation / Extraktion / Routing)
    if aufgabe["typ"] in ("klassifizieren", "extrahieren", "routen", "zusammenfassen"):
        return "claude-haiku-4-5"
    
    # Tier 2 — Sonnet (Standard-Produktion)
    if aufgabe["komplexitaet"] < 8 and aufgabe["autonome_minuten"] < 10:
        return "claude-sonnet-4-6"
    
    # Tier 3 — Opus (gerechtfertigt durch Fehlerkosten)
    if (aufgabe["komplexitaet"] >= 8
        or aufgabe["autonome_minuten"] >= 10
        or aufgabe["domaine"] in ("recht", "finanzen", "multi_file_refactor")):
        return "claude-opus-4-8"
    
    return "claude-sonnet-4-6"  # sicherer Standard
```

### Effort-Stufen (Sonnet 4.6 / Opus 4.8)

```python
# effort ersetzt budget_tokens — jetzt allgemein verfügbar
effort_karte = {
    "einfache_qa": "low",
    "standard_coding": "medium",
    "komplexe_analyse": "high",   # Standard für Opus 4.6+
    "frontier_agent": "max",      # Nur Opus 4.8
    # "xhigh" nur in Claude Code verfügbar
}

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=4096,
    thinking={"type": "enabled", "effort": "high"},
    messages=[...]
)
```

> ⚠️ `effort: high` auf Sonnet 4.6 kann mehr Latenz erzeugen als Sonnet 4.5, falls vorher unkonfiguriert. P95 benchmarken vor dem Rollout.


## Kostenmodellierung — Beispielrechnungen

### Beispiel 1: Hochvolumige Klassifikations-Pipeline

```
Aufgabe: 1 Mio. Kundensupport-Tickets pro Monat klassifizieren
Komplexität: Niedrig (strukturierter Input → feste Taxonomie)
Modell: claude-haiku-4-5

Geschätzte Token pro Anfrage: ~800 in + ~50 out
Monatliche Kosten:
  Input:  1.000.000 × 800 / 1.000.000 × $1,00  = $800
  Output: 1.000.000 × 50  / 1.000.000 × $5,00  = $250
  Gesamt: ~$1.050/Monat

Mit Prompt-Caching (wiederholter System-Prompt, ~500 Token):
  Cache-Lesevorgänge bei 0,1× → $800 × 0,1 = $80 Input
  Neuer Gesamt: ~$330/Monat  (69% Ersparnis)
```

### Beispiel 2: Autonomer Coding-Agent

```
Aufgabe: Multi-File-Refactor, 50 Dateien, ~10 Min. autonome Ausführung
Modell: claude-opus-4-8

Geschätzte Token pro Session: ~80K in + ~20K out
Tägliche Sessions: 50 Entwickler × 3 Sessions = 150
Monatliche Kosten (22 Arbeitstage):
  Input:  150 × 22 × 80K / 1M × $5,00  = $1.320
  Output: 150 × 22 × 20K / 1M × $25,00 = $1.650
  Gesamt: ~$2.970/Monat

Hinweis: Anthropic berichtet durchschnittlich ~$13/Entwickler/aktivem Tag
für Claude Code, 90% der Nutzer unter $30/Tag.
```


## Veraltete Modell-Referenz

| Modell-ID | Status | Nachfolger | Alter Preis |
|---|---|---|---|
| `claude-sonnet-4-20250514` | ❌ Pensioniert 15. Juni 2026 | `claude-sonnet-4-6` | $3 / $15 |
| `claude-opus-4-20250514` | ❌ Pensioniert 15. Juni 2026 | `claude-opus-4-8` | $15 / $75 |
| `claude-3-5-haiku-20241022` | ❌ Bereits pensioniert | `claude-haiku-4-5` | $0,80–$1 / $4 |

> **Opus 4.8 ($5/$25) bedeutet eine 67% Preissenkung gegenüber Opus 4 ($15/$75)** — die größte Kostenreduktion in der Claude-4.x-Familie.


## Plan-Tier nach Teamgröße

| Szenario | Empfohlener Plan |
|---|---|
| Solo-Entwickler, ≤10 tägliche Sessions | Pro ($17–20/Monat) |
| Pro-Session-Limit-Warnungen 3×+ pro Woche | Max ($100–200/Monat) |
| 3+ Personen mit gemeinsamen Projects/Abrechnung | Team Standard ($20/Platz/Monat, min. 5 Plätze) |
| Engineering-Team mit hohem Claude-Code-Verbrauch | Team Premium ($100/Platz/Monat, min. 5 Plätze) |
| SCIM, Audit-Logs, HIPAA/BAA, IP-Allowlisting, >150 Plätze benötigt | Enterprise ($20/Platz + API-Nutzung, min. 20 Self-Serve) |

> **Kontextfenster-Klarstellung:** „500K Kontext ist Enterprise-exklusiv" ist falsch. Laut Anthropic-Supportdokumenten ist 500K auf **allen bezahlten Plänen** verfügbar für Opus 4.6+/Sonnet 4.6 im Chat.


*← [Advanced Prompting](./advanced-prompting.md) | Weiter: [Projects & Artifacts →](./projects-artifacts.md)*
