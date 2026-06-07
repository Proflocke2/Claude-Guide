# 🧠 Claude Master Guide — Deutsch

> Senior-Referenz für Claude API, Prompt Engineering, Projects, Tools und Enterprise-Einsatz.  
> Alle Inhalte spiegeln den **Juni 2026** Produktionsstand der Claude API wider.

---

## 📚 Kapitel

| # | Kapitel | Inhalt |
|---|---|---|
| 1 | [Advanced Prompt Engineering](./advanced-prompting.md) | XML-Tags, Chain-of-Thought, Structured Outputs |
| 2 | [Modell-Auswahlmatrix](./model-matrix.md) | Opus vs. Sonnet vs. Haiku — Kosten, Latenz, Fähigkeiten |
| 3 | [Projects & Artifacts](./projects-artifacts.md) | Wissensdatenbanken, Kontextfenster-Optimierung, Artifact-Typen |
| 4 | [Custom Instructions & System Prompts](./custom-instructions.md) | Vorlagen, Role Prompting, persistente Konfigurationen |
| 5 | [Tools & Konnektoren](./tools-connectors.md) | Tool Use, MCP, Function Calling, Built-in Tools |

---

## 🚨 Kritische Migration

```
claude-opus-4-20250514   →  claude-opus-4-8
claude-sonnet-4-20250514 →  claude-sonnet-4-6
claude-3-5-haiku-20241022 → claude-haiku-4-5  (bereits deaktiviert)
```

**Deadline: 15. Juni 2026.** Die API wirft danach einen Hard Failure — kein Fallback, kein Auto-Routing. Durchsuche das gesamte Monorepo:

```bash
rg "20250514" .
rg "claude-3-5-haiku" .
rg "budget_tokens" .       # veraltet — nutze effort: high|medium|low|max
```

---

## 🔑 API-Änderungen in 4.6 / 4.7 / 4.8

| Änderung | Altes Verhalten | Neues Verhalten |
|---|---|---|
| **Prefilling** | Assistant-Turn mit `{` befüllen für JSON | Structured Outputs (`output_config.format`) |
| **Sampling-Parameter** | `temperature`, `top_p`, `top_k` frei setzbar | **400-Fehler** auf Opus 4.7+ bei Nicht-Standard |
| **Thinking Budget** | `budget_tokens` Beta-Header | `effort: high \| medium \| low \| max` |
| **Batch API** | 2× Aufpreis für erweiterte Ausgabe | Flat-Preis, 50% Rabatt, 300K-Output via Beta-Header |
| **Token-Output** | — | Neuer Tokenizer erzeugt bis zu 35% mehr Token |

---

## 💡 Fünf Grundsätze zum Einstieg

1. **XML-Struktur zuerst.** Claude wurde auf XML-Tags als Prompt-Gliederungsprimitive trainiert.
2. **`system` nur für Rolle + persistenten Kontext.** Aufgaben-spezifische Anweisungen → User-Turn.
3. **Structured Outputs, kein Prefilling.** Prefilling gibt 400 auf aktuellen Modellen.
4. **Dokumente → oben. Query → unten.** Anthropic meldet bis zu 30% Qualitätsgewinn.
5. **Tool-Beschreibungen wie Docstrings schreiben.** Die Beschreibung entscheidet, wann das Modell aufruft.

---

*← [Zurück zur Wurzel](../README.md)*
