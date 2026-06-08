> **Version:** v1.2.0 · Stand: Juni 2026 · [Changelog](../../CHANGELOG.md)

# 🧠 Claude Master Guide — Deutsch

> Senior-Referenz für Claude API, Prompt Engineering, Projects, Tools und Enterprise-Einsatz.  
> Alle Inhalte spiegeln den **Juni 2026** Produktionsstand der Claude API wider.


## 📚 Kapitel

| # | Kapitel | Inhalt |
|---|---|---|
| 1 | [Advanced Prompt Engineering](./advanced-prompting.md) | XML-Tags, Chain-of-Thought, Structured Outputs |
| 2 | [Modell-Auswahlmatrix](./model-matrix.md) | Opus vs. Sonnet vs. Haiku — Kosten, Latenz, Fähigkeiten |
| 3 | [Projects & Artifacts](./projects-artifacts.md) | Wissensdatenbanken, Kontextfenster-Optimierung, Artifact-Typen |
| 4 | [Custom Instructions & System Prompts](./custom-instructions.md) | Vorlagen, Role Prompting, persistente Konfigurationen |
| 5 | [Tools & Konnektoren](./tools-connectors.md) | Tool Use, MCP, Function Calling, Built-in Tools |


## 🚨 Kritische Migration

```
claude-opus-4-20250514   →  claude-opus-4-8
claude-sonnet-4-20250514 →  claude-sonnet-4-6
claude-3-5-haiku-20241022 → claude-haiku-4-5-20251001  (bereits deaktiviert)
```

Deadline: 15. Juni 2026.** Die API wirft danach einen Hard Failure — kein Fallback, kein Auto-Routing. Durchsuche das gesamte Monorepo:

```bash
rg "20250514" .
rg "claude-3-5-haiku" .
rg "budget_tokens" .       # veraltet — nutze effort: high|medium|low|max
```


## 🔑 API-Änderungen in 4.6 / 4.7 / 4.8

| Änderung | Altes Verhalten | Neues Verhalten |
|---|---|---|
| **Prefilling** | Assistant-Turn mit `{` befüllen für JSON | Structured Outputs (primär für Opus 4.7+, modellabhängig) (`output_config.format`) |
| **Sampling-Parameter** | `temperature`, `top_p`, `top_k` frei setzbar | **400-Fehler** auf Opus 4.7+ bei Nicht-Standard |
| **Thinking Budget** | `budget_tokens` Beta-Header | `thinking: {type: "adaptive"}` + `output_config: {effort: "low\|medium\|high\|xhigh\|max"}` |
| **Batch API** | 2× Aufpreis für erweiterte Ausgabe | Flat-Preis, 50% Rabatt, 300K-Output via Beta-Header |
| **Token-Output** | — | Neuer Tokenizer erzeugt bis zu 35% mehr Token |


## 💡 Fünf Grundsätze zum Einstieg

1. **XML-Struktur zuerst.** Claude wurde auf XML-Tags als Prompt-Gliederungsprimitive trainiert.
2. **`system` nur für Rolle + persistenten Kontext.** Aufgaben-spezifische Anweisungen → User-Turn.
3. **Structured Outputs, kein Prefilling.** Prefilling gibt 400 auf aktuellen Modellen.
4. **Dokumente → oben. Query → unten.** Anthropic meldet bis zu 30% Qualitätsgewinn.
5. **Tool-Beschreibungen wie Docstrings schreiben.** Die Beschreibung entscheidet, wann das Modell aufruft.


*← [Zurück zur Wurzel](../README.md)*

---

> **⚖️ Rechtliche Hinweise**
>
> **Marken-Disclaimer:** Dieser Guide ist unabhängig erstellt und steht in keiner Verbindung zu Anthropic PBC. Claude™ ist eine Marke von Anthropic. Alle weiteren Marken gehören ihren jeweiligen Inhabern.
>
> **Datenschutz:** Consumer-Nutzer (kostenlos / Pro) sollten beachten, dass Claude-Gespräche standardmäßig zur Modellverbesserung genutzt werden können. Seit der Policy-Änderung im August 2025 ist ein aktiver Opt-out in den Einstellungen erforderlich. „Vertraulich" im Projektkontext bedeutet nicht automatisch, dass Anthropic keinen Zugriff hat — Enterprise-Pläne bieten hier stärkere Garantien.
>
> **Humanizer & KI-Faktencheck:** KI-generierte Faktenchecks ersetzen keine professionelle Prüfung. Der Einsatz von Humanizern im akademischen Kontext kann gegen Hochschul- oder Prüfungsrichtlinien verstoßen — Eigenverantwortung liegt beim Nutzer.
