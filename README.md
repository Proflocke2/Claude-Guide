# 🧠 Claude Ultimate Guide

> **Der komplette Claude-Guide — für Einsteiger und Entwickler.**  
> Basiert auf offizieller Anthropic-Dokumentation — Stand Juni 2026.


## 🌐 Sprache wählen / Choose Your Language

<div align="center">

| 🇩🇪 Deutsch | 🇬🇧 English |
|:---:|:---:|
| [**→ Alltags-Guide**](./everyday/de/README.md) | [**→ Everyday Guide**](./everyday/en/README.md) |
| Für alle — kein Tech-Wissen nötig | For everyone — no tech knowledge needed |
| [**→ Developer-Guide**](./dev/de/README.md) | [**→ Developer Guide**](./dev/en/README.md) |
| API, Modelle, MCP, Code-Beispiele | API, models, MCP, code examples |

</div>


## 📁 Struktur des Repositories

```
claude-ultimate-guide/
├── README.md                        ← Du bist hier / You are here
│
├── everyday/                        ← Für alle — kein Tech-Wissen nötig
│   ├── de/
│   │   ├── README.md                ← Setup, Hacks, Schreibstil
│   │   └── vorlagen.md              ← Kopierfertige Prompts & Configs
│   └── en/
│       ├── README.md                ← Setup, hacks, writing style
│       └── templates.md             ← Copy-paste prompts & configs
│
└── dev/                             ← Für Entwickler — API & Code
    ├── de/
    │   ├── README.md                ← Übersicht & Migration
    │   ├── advanced-prompting.md    ← XML-Tags, CoT, Structured Outputs
    │   ├── model-matrix.md          ← Opus vs Sonnet vs Haiku
    │   ├── custom-instructions.md   ← System-Prompt-Vorlagen
    │   ├── projects-artifacts.md    ← Projects, Artifacts, Storage API
    │   └── tools-connectors.md      ← Tool Use, MCP, Function Calling
    └── en/
        ├── README.md                ← Overview & migration
        ├── advanced-prompting.md    ← XML tags, CoT, Structured Outputs
        ├── model-matrix.md          ← Opus vs Sonnet vs Haiku
        ├── custom-instructions.md   ← System prompt templates
        ├── projects-artifacts.md    ← Projects, Artifacts, Storage API
        └── tools-connectors.md      ← Tool use, MCP, Function Calling
```


## ⚡ Quick Facts — Juni 2026

| Modell | Kontext | Output | Preis (in/out pro MTok) |
|---|---|---|---|
| `claude-opus-4-8` | 1M Token | 128K | $5 / $25 |
| `claude-sonnet-4-6` | 1M Token | 64K | $3 / $15 |
| `claude-haiku-4-5` | 200K Token | 64K | $1 / $5 |

> ⚠️ **`claude-sonnet-4-20250514` und `claude-opus-4-20250514` werden am 15. Juni 2026 deaktiviert.** Hard API Failure — kein Auto-Routing.


## 🔗 Offizielle Quellen / Official Resources

- [Anthropic Docs](https://docs.claude.com)
- [Prompt Engineering Overview](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview)
- [API Reference](https://platform.claude.com)
- [Support](https://support.claude.com)
