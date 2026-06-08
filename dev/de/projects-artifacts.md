# Projects & Artifacts

> Kapitel 3 des Claude Master Guide — Deutsch  
> Themen: Projects-Architektur, Kontextfenster-Optimierung, Artifact-Typen und Fähigkeiten.


## Claude Projects

Projects sind persistente Arbeitsbereiche, die **(1) eine Wissensdatenbank, (2) Custom Instructions und (3) Chat-Historie** kombinieren — alles bleibt sitzungsübergreifend erhalten.

### Dateilimits

| Typ | Limit |
|---|---|
| Unterstützte Formate | PDF, DOCX, TXT, CSV, HTML, ODT, RTF, EPUB; Bilder bis 30 MB / 8000×8000 px |
| Chat-Interface (außerhalb Projects) | Max. 20 Dateien pro einzelner Konversation |
| Files API (Developer-Tier) | Bis zu 5 GB serverseitig, per ID referenziert |
| PDF-Visuelle Analyse | Vollständig (Text + Charts + Bilder) unter 100 Seiten; Text-Only-Fallback darüber |

### Kontextfenster in Projects

| Modell | Standard-Kontext | Chat mit Opus 4.6+ / Sonnet 4.6 |
|---|---|---|
| Alle bezahlten Pläne | 200K | 500K |
| Claude Code | 1M (Credits erforderlich bei Pro/Team; keine Beschränkung bei nutzungsbasiertem Enterprise) |

> **Häufiges Missverständnis:** „500K Kontext ist Enterprise-exklusiv" ist falsch. Laut Anthropics Supportdokumenten ist 500K auf **allen bezahlten Plänen** verfügbar (laut Anthropic-Supportdokumenten) verfügbar beim Chatten mit Opus 4.6+/Sonnet 4.6.

### RAG-Auto-Trigger

Projects aktivieren automatisch Retrieval-Augmented Generation (RAG), wenn das Projektwissen sich dem Kontextfenster-Limit nähert — erweitert die effektive Kapazität um bis zu **10×** via `project_knowledge_search`-Tool.

> ⚠️ **Bekannter Bug** (GitHub Issue #25759 auf anthropics/claude-code): RAG-Modus scheint bei **~13 Dateien** zu aktivieren, unabhängig von der Token-Gesamtanzahl — sogar bei ~2% der angegebenen Kapazität. Bei vielen kleinen Dateien ist RAG-Retrieval früher als erwartet zu rechnen.

### Wann Projects vs. Externer Speicher

| Szenario | Verwenden |
|---|---|
| ≤~30 Dateien, unter 30 MB, bezahlter Plan | **Projects** |
| Dateien überschreiten regelmäßig 30 MB | Externer Speicher + MCP |
| Gleiche Wissensdatenbank über mehrere KI-Clients | Externer Speicher + MCP |
| Agenten benötigen programmatischen Dateizugriff | Externer Speicher + MCP |
| Inhalte, die mehr als einmal referenziert werden | **Projects** (gecachter Inhalt verbraucht kein Message-Budget bei Wiederverwendung) |

### Project-Optimierungs-Checkliste

```
✅ Project-Instructions KURZ halten — aufgaben-spezifische Instructions in den Chat
✅ Nicht verwendete Dateien entfernen (besonders in Claude Code — MCP-Tool-Definitionen sind token-schwer)
✅ Extended Thinking ausschalten, wenn nicht benötigt
✅ Projects für wiederholend referenzierte Inhalte nutzen (Caching-Vorteil)
✅ Für bezahlte Pläne mit Code Execution: Server-seitige Kompaktierung verwaltet ältere Nachrichten automatisch
✅ Große monolithische Dokumente in kleinere thematische Dateien aufteilen (bessere RAG-Retrievals)
```


## Artifacts

Artifacts sind eigenständige, editierbare, iterierbare Inhaltsfenster, die neben dem Chat öffnen. Anthropic löst ein Artifact aus, wenn Inhalt *„signifikant und eigenständig ist, typischerweise über 15 Zeilen"* und etwas, das wahrscheinlich bearbeitet, iteriert oder außerhalb der Konversation wiederverwendet wird.

### Artifact-Typen

| Typ | Beste für | Hinweise |
|---|---|---|
| **Dokumente** | Markdown-Berichte, SOPs, Briefs, strukturierte Docs | Standard für Langform-Schreiben |
| **Code** | Skripte in jeder Sprache, ausführbare Snippets | Syntax-hervorgehoben, in einigen Kontexten ausführbar |
| **Interaktiv** | Rechner, Quizze, Dashboards, Spiele | Leistungsstärkste Kategorie — volles HTML/JS |
| **SVG** | Logos, Icons, technische Illustrationen | Inline-Rendering |
| **Diagramme** | Flussdiagramme, Sequenzdiagramme, Prozessmaps | Mermaid-basiert |
| **React-Komponenten** | Vollständige Single-File-UIs mit Tailwind, lucide-react, recharts, shadcn/ui | Produktionsqualität UI-Prototyping |

### React-Artifact — Verfügbare Bibliotheken

```javascript
// Verfügbare Imports in React-Artifacts (kein Build-Schritt)
import { useState, useEffect, useReducer, useCallback, useMemo } from "react"
import { LineChart, BarChart, XAxis, YAxis, Tooltip, Legend } from "recharts"
import { AlertCircle, Check, ChevronRight } from "lucide-react"  // lucide-react@0.383.0
import _ from 'lodash'
import * as d3 from 'd3'
import * as math from 'mathjs'
import Papa from 'papaparse'      // CSV-Verarbeitung
import * as XLSX from 'xlsx'      // Excel-Verarbeitung
import * as Tone from 'tone'      // Audio
import * as THREE from 'three'    // 3D (r128; kein OrbitControls)

// Tailwind: nur Core-Utility-Klassen (vordefinierto Base-Stylesheet)
// shadcn/ui: import { Button } from '@/components/ui/button'
// Externe Skripte: nur https://cdnjs.cloudflare.com
```

> ⚠️ **Kein localStorage/sessionStorage** in Artifacts — nicht unterstützt, schlägt stumm fehl. React `useState` für In-Session-State nutzen.

### Artifact Persistent Storage API

Für zustandsbehaftete Artifacts (Tagebücher, Tracker, Bestenlisten):

```javascript
// Key-Value-Speicher persistiert sitzungsübergreifend
// Persönliche Daten (Standard: shared=false)
await window.storage.set('eintraege:2026-06-07', JSON.stringify(eintrag));
const ergebnis = await window.storage.get('eintraege:2026-06-07');
const eintrag = ergebnis ? JSON.parse(ergebnis.value) : null;

// Geteilte Daten (für alle Nutzer des Artifacts sichtbar)
await window.storage.set('bestenliste:alice', JSON.stringify(punktzahl), true);

// Schlüssel auflisten
const schluessel = await window.storage.list('eintraege:');

// Regeln:
// - Schlüssel: unter 200 Zeichen, kein Leerzeichen / Schrägstriche / Anführungszeichen
// - Werte: unter 5 MB, nur Text/JSON
// - Rate-limitiert — verwandte Daten in einzelne Schlüssel bündeln
// - Artifact muss veröffentlicht sein für geteilten Speicher
```

**Schlüssel-Design-Pattern:** Zusammen aktualisierte Daten in einzelne Schlüssel kombinieren:

```javascript
// ✅ Gut — ein Roundtrip
await window.storage.set('nutzer-status', JSON.stringify({
    karten, vorteile, abschluss, zuletzt_aktualisiert
}));

// ❌ Schlecht — sequentielle Roundtrips treffen Rate-Limits
await window.storage.set('karten', JSON.stringify(karten));
await window.storage.set('vorteile', JSON.stringify(vorteile));
await window.storage.set('abschluss', JSON.stringify(abschluss));
```

### Eingebettete Claude API in Artifacts (Pro/Max/Team/Enterprise)

```javascript
// Innerhalb eines Artifacts (React oder HTML)
const antwort = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
        model: "claude-sonnet-4-6",
        max_tokens: 1000,
        messages: [{ role: "user", content: nutzerEingabe }]
    })
});
const daten = await antwort.json();
const text = daten.content
    .filter(block => block.type === "text")
    .map(block => block.text)
    .join("\n");
```


*← [Modell-Matrix](./model-matrix.md) | Weiter: [Custom Instructions →](./custom-instructions.md)*
