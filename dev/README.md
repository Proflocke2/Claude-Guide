> **Version:** v1.2.0 · Stand: Juni 2026 · [Changelog](../../CHANGELOG.md)

# 🧠 Claude Master Guide

> **Production-grade reference for Claude API, Prompt Engineering, and Enterprise deployment.**  
> Based on official Anthropic documentation — updated June 2026.


## 🌐 Choose Your Language / Sprache wählen

<div align="center">

| 🇬🇧 English | 🇩🇪 Deutsch |
|:---:|:---:|
| [**→ English Guide**](./en/README.md) | [**→ Deutscher Guide**](./de/README.md) |
| Full reference in English | Vollständige Referenz auf Deutsch |

</div>


## 📁 Repository Structure

```
claude-master-guide/
├── README.md                    ← You are here
├── en/
│   ├── README.md                ← English master overview
│   ├── advanced-prompting.md    ← XML tags, CoT, Structured Outputs
│   ├── model-matrix.md          ← Opus vs Sonnet vs Haiku decision guide
│   ├── custom-instructions.md   ← System prompt templates & configs
│   ├── projects-artifacts.md    ← Projects, context window, Artifacts
│   └── tools-connectors.md     ← Tool use, MCP, Function Calling
└── de/
    ├── README.md                ← Deutscher Gesamtüberblick
    ├── advanced-prompting.md    ← XML-Tags, CoT, Structured Outputs
    ├── model-matrix.md          ← Opus vs Sonnet vs Haiku Entscheidungsguide
    ├── custom-instructions.md   ← System-Prompt-Vorlagen & Configs
    ├── projects-artifacts.md    ← Projects, Kontextfenster, Artifacts
    └── tools-connectors.md     ← Tool Use, MCP, Function Calling
```


## ⚡ Quick Facts — June 2026

| Model | Context | Output | Price (in/out per MTok) |
|---|---|---|---|
| `claude-opus-4-8` | 1M tokens | 128K | $5 / $25 |
| `claude-sonnet-4-6` | 1M tokens | 64K | $3 / $15 |
| `claude-haiku-4-5-20251001` | 200K tokens | 64K | $1 / $5 |

> ⚠️ **`claude-sonnet-4-20250514` and `claude-opus-4-20250514` retire June 15, 2026.** Hard API failure — no auto-routing.


## 🔗 Official Resources

- [Anthropic Docs](https://docs.claude.com)
- [Prompt Engineering Overview](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview)
- [API Reference](https://platform.claude.com)
- [Support](https://support.claude.com)
