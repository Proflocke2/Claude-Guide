# Changelog

All notable changes to this project are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.3.0] — 2026-06-08

### Fixed
- Haiku Model ID: `claude-haiku-4-5-20251001` → `claude-haiku-4-5-20251001` (alle Dateien)
- `effort`-Parameter-Syntax: `thinking: {type: "adaptive"}` + `output_config: {effort: ...}` (war falsch in `thinking`-Block)
- Doppelter Text in `dev/de/model-matrix.md` (500K-Kontextsatz)
- Unbelegt attributierte Zitate als Community-Erfahrungswert markiert (40%-Halluzinations-Claim, 30%-Qualitätsclaim, 20–40%-Konsistenz-Claim)
- Erfundenes Direktzitat in `dev/en/advanced-prompting.md` entfernt und paraphrasiert

### Added
- Section 7 "Das Think-Tool" in `dev/de/advanced-prompting.md` (war nur in EN vorhanden)
- `CONTRIBUTING.md` jetzt bilingual (DE + EN)
- LICENSE von MIT auf CC BY 4.0 geändert (passender für Dokumentation)

## [1.2.0] — 2026-06-08

### Added
- `everyday/de/quick-setup.md` — Startklar in 30 Min. inkl. 50-Fragen-Onboarding-Prompt
- `everyday/en/quick-setup.md` — Ready in 30 min including 50-question onboarding prompt
- `SOURCES.md` — alle 13 offiziellen Anthropic-Quellen zentral verlinkt
- `LICENSE` (MIT), `CHANGELOG.md`, `CONTRIBUTING.md`
- Inline-Quellverlinkungen an relevanten Fakten in allen Kapiteln
- Erweiterter Haftungsausschluss für Preis- und Modelländerungen
- Versionsstand (`v1.2.0 · Juni 2026`) in allen README-Headern

### Fixed
- Sonnet 4.6 Computer Use Benchmark: 61,4% → **72,5%** (OSWorld-Verified)
- Opus 4.8 Benchmarks klar getrennt: **83,4% OSWorld-Verified** / **84% Online-Mind2Web**
- Prefilling-Verhalten als modellabhängig (primär Opus 4.7+) deklariert
- Wissens-Cutoff von pauschal "August 2025" auf modellspezifisch korrigiert
- UI-Pfad für Personal Preferences korrigiert (Initialen → Profil)
- 500K-Kontext-Fehler behoben: verfügbar auf allen bezahlten Plänen
- Haiku 3.5 Retirement-Datum präzisiert: Februar 2026
- Leere `{de,en}`-Ordner entfernt

### Changed
- XML-Statistik von "20–40% konsistenter" auf belegte Formulierung angepasst
- Alle Kapitel mit direkten Quellenlinks an relevanten Fakten versehen
- Disclaimer um Preisänderungs-Haftungsausschluss erweitert

---

## [1.1.0] — 2026-06-07

### Added
- Deutsches und englisches Everyday-Guide (`/everyday/de/`, `/everyday/en/`)
- Developer-Guide mit 5 Kapiteln je Sprache (`/dev/de/`, `/dev/en/`)
- Humanizer-Prompts, Verbotsliste, Voice-Matching
- Fakten-Checker-Prompt, Halluzinations-Schutzformeln
- Rechtliche Disclaimers (Marke, Datenschutz, Humanizer)
- RAG-Bug-Warnung (GitHub Issue #25759)
- localStorage-Warnung in Artifact-Kapitel

---

## [1.0.0] — 2026-06-06

### Added
- Initiales Repository mit bilingualer Struktur (DE/EN)
- Developer-Guide: Prompt Engineering, Modell-Matrix, Projects & Artifacts, System Prompts, Tools & MCP
- Everyday-Guide: Setup, XML-Tricks, Schreibstil, Halluzinations-Schutz
- Vorlagen und Templates zum Kopieren
