# Tools & Konnektoren

> Kapitel 5 des Claude Master Guide — Deutsch  
> Themen: Tool-Use-Architektur, MCP, Built-in Tools, Parallele Ausführung, Strict Schemas.


## Tool Use — API-Architektur

Anders als OpenAIs separater `tool`-Role bettet Claude Tools direkt in die User/Assistant-Nachrichtenstruktur ein:

- **Assistant-Nachrichten:** enthalten `text`- und `tool_use`-Blöcke
- **User-Nachrichten:** enthalten `text`- und `tool_result`-Blöcke

**Kritische Reihenfolgeregel:** `tool_result`-Blöcke müssen **vor** allen `text`-Blöcken in der User-Nachricht kommen. Text vor tool_result gibt einen 400-Fehler.

```python
# Korrekte Konversationsstruktur
nachrichten = [
    {"role": "user", "content": "Wie ist das Wetter in Berlin und Paris?"},
    {
        "role": "assistant",
        "content": [
            {"type": "text", "text": "Ich prüfe beide Städte für dich."},
            {"type": "tool_use", "id": "aufruf_1", "name": "wetter_holen",
             "input": {"stadt": "Berlin"}},
            {"type": "tool_use", "id": "aufruf_2", "name": "wetter_holen",
             "input": {"stadt": "Paris"}}
        ]
    },
    {
        "role": "user",
        "content": [
            # tool_result VOR text — zwingend erforderlich
            {"type": "tool_result", "tool_use_id": "aufruf_1",
             "content": [{"type": "text", "text": "18°C, bewölkt"}]},
            {"type": "tool_result", "tool_use_id": "aufruf_2",
             "content": [{"type": "text", "text": "22°C, sonnig"}]},
        ]
    }
]
```


## Tool-Definition — Best Practices

Ein Tool erfordert drei Felder. Die **Beschreibung ist das kritischste** — sie bestimmt, wann das Modell aufruft.

```python
tools = [
    {
        "name": "github_prs_auflisten",
        "description": """
Ruft offene Pull Requests aus einem GitHub-Repository ab.

VERWENDEN WENN:
- Nutzer nach ausstehenden PRs, Code-Reviews oder der Merge-Queue fragt
- Nutzer den Status offener Arbeit prüfen möchte
- Nutzer sagt "was wartet auf Review" oder Ähnliches

NICHT VERWENDEN WENN:
- Nutzer nach gemergten oder geschlossenen PRs fragt (stattdessen github_geschlossene_prs_auflisten)
- Nutzer nach Issues fragt, nicht PRs
- Bereits aktuelle PR-Daten aus den letzten 5 Minuten im Kontext vorhanden
        """.strip(),
        "input_schema": {
            "type": "object",
            "properties": {
                "besitzer": {
                    "type": "string",
                    "description": "GitHub-Organisation oder Benutzername"
                },
                "repo": {
                    "type": "string",
                    "description": "Repository-Name ohne Besitzer-Präfix"
                },
                "status": {
                    "type": "string",
                    "enum": ["open", "closed", "all"],
                    "description": "PR-Status-Filter"
                },
                "limit": {
                    "type": "integer",
                    "minimum": 1,
                    "maximum": 100,
                    "description": "Max. zurückzugebende Ergebnisse (Standard: 30)"
                }
            },
            "required": ["besitzer", "repo"]
        }
    }
]
```

### Tool Choice Control

```python
# auto (Standard) — Claude entscheidet, wann aufzurufen
tool_choice = {"type": "auto"}

# any — Claude zum Aufrufen mindestens eines Tools zwingen
tool_choice = {"type": "any"}

# tool — ein spezifisches Tool erzwingen (Structured-Output-as-Tool-Pattern)
tool_choice = {"type": "tool", "name": "ergebnis_zurueckgeben"}

# none — alle Tools deaktivieren (spart ~System-Prompt-Token für diese Anfrage)
tool_choice = {"type": "none"}
```

### Strict Schema Mode

`strict: True` aktiviert grammar-constrained Sampling — Tool-Inputs entsprechen **garantiert** dem JSON Schema:

```python
{
    "name": "rechnung_erstellen",
    "description": "Erstellt einen neuen Rechnungsdatensatz.",
    "input_schema": {
        "type": "object",
        "properties": {
            "kunden_id": {"type": "string"},
            "betrag_cent": {"type": "integer", "minimum": 1},
            "waehrung": {"type": "string", "enum": ["EUR", "USD", "GBP"]},
            "faelligkeitsdatum": {"type": "string", "format": "date"}
        },
        "required": ["kunden_id", "betrag_cent", "waehrung", "faelligkeitsdatum"]
    },
    "strict": True  # garantierte Schema-Übereinstimmung
}
```

> **Strict-Mode-Limits:** Kombiniertes ~24-Parameter-Limit über alle Strict-Schemas pro Anfrage. Inkompatibel mit Citations.


## Parallele Tool-Ausführung

Auf allen 4.x-Modellen unterstützt, standardmäßig aktiviert. Sonnet 4.5+ ist besonders effizient.

```python
def agenten_loop_ausführen(nachrichten: list, tools: list) -> str:
    while True:
        antwort = client.messages.create(
            model="claude-sonnet-4-6",
            max_tokens=4096,
            tools=tools,
            messages=nachrichten
        )

        if antwort.stop_reason == "end_turn":
            return next(b.text for b in antwort.content if b.type == "text")

        if antwort.stop_reason == "tool_use":
            # Alle Tool-Aufrufe sammeln (können parallel sein)
            tool_aufrufe = [b for b in antwort.content if b.type == "tool_use"]

            # Alle parallel ausführen
            ergebnisse = []
            for aufruf in tool_aufrufe:
                ergebnis = tool_ausführen(aufruf.name, aufruf.input)
                ergebnisse.append({
                    "type": "tool_result",
                    "tool_use_id": aufruf.id,
                    "content": [{"type": "text", "text": str(ergebnis)}]
                })

            nachrichten.append({"role": "assistant", "content": antwort.content})
            nachrichten.append({"role": "user", "content": ergebnisse})
```


## Built-in Anthropic Tools

Laufen auf Anthropic-Infrastruktur — nur `type`-Feld erforderlich, kein Custom Schema:

| Tool | Beschreibung | Preisgestaltung |
|---|---|---|
| `web_search` | Echtzeit-Websuche | Nutzungsbasiert pro Suche |
| `web_fetch` | Bestimmten URL-Inhalt abrufen | Inklusive |
| `code_execution` | Sandboxed Python-Ausführung | Inklusive |
| `bash` | Shell-Zugriff | Inklusive |
| `text_editor` | Dateibearbeitungsoperationen | Inklusive |
| `computer_use` | Desktop-Steuerung (61,4%–84% OSWorld-Verified auf Sonnet 4.5+/4.6) | Inklusive |
| `memory` | Persistenter Agenten-Speicher | Inklusive |
| `advisor` | Schnellen Executor mit klügerem Advisor mid-generation pairen | Public Beta |

```python
# Built-in Tools aktivieren
antwort = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    tools=[
        {"type": "web_search_20250305", "name": "web_search"},
        {"type": "code_execution_20250522", "name": "code_execution"},
        {"type": "bash_20250124", "name": "bash"}
    ],
    messages=[{"role": "user", "content": "Suche nach den neuesten React-19-Release-Notes und führe dann einen schnellen Benchmark durch."}]
)
```


## MCP (Model Context Protocol)

Offener Standard zum Verbinden beliebiger MCP-kompatibler Clients (Claude.ai, Claude Code, Claude Desktop, VS Code, Drittanbieter-Agenten) mit beliebigen MCP-Servern via JSON-RPC 2.0.

**Drei Transporte:** `stdio` (lokaler Subprocess), `SSE`, `Streamable HTTP`

### Konfiguration (`.mcp.json`)

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "internes-system": {
      "url": "https://mcp-server.intern/sse",
      "transport": "sse"
    }
  }
}
```

### MCP-Sicherheits-Checkliste

```
✅ ${ENV_VAR}-Referenzen in .mcp.json verwenden — niemals Secrets hardcoden
✅ Claude Code v2.1+ unterstützt per-Tool Allow/Deny-Listen — nutzen
✅ @modelcontextprotocol/* und vertrauenswürdige Community-Pakete bevorzugen
✅ stdio-Server laufen mit Datei-System-Berechtigungen des Users — Scope eingrenzen
✅ Claude Code Tools nach Intention auswählen lassen (Beschreibung ist das Ranking-Signal)
✅ Standard-Output-Cap: 25.000 Token pro MCP-Tool-Aufruf (Warnung bei 10.000)
✅ Mit MAX_MCP_OUTPUT_TOKENS oder per-Tool anthropic/maxResultSizeChars konfigurieren
✅ Nicht verwendete Server deaktivieren für schlanken Kontext
```

### MCP-Antworten verarbeiten (Artifact / API-Kontext)

```javascript
async function mitMCPAufrufen(nutzernachricht, mcpServer) {
    const antwort = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
            model: "claude-sonnet-4-6",
            max_tokens: 1000,
            messages: [{ role: "user", content: nutzernachricht }],
            mcp_servers: mcpServer
        })
    });

    const daten = await antwort.json();

    // Nach Typ extrahieren — niemals nach Position
    const toolErgebnisse = daten.content
        .filter(item => item.type === "mcp_tool_result")
        .map(item => {
            try {
                return JSON.parse(item.content?.[0]?.text || "{}");
            } catch {
                return item.content?.[0]?.text || "";
            }
        });

    const textAntwort = daten.content
        .filter(item => item.type === "text")
        .map(item => item.text)
        .join("\n");

    const toolAufrufe = daten.content
        .filter(item => item.type === "mcp_tool_use")
        .map(item => ({ name: item.name, input: item.input }));

    return { textAntwort, toolErgebnisse, toolAufrufe };
}
```

### Lazy Tool Loading (Agenten mit 100+ Tools)

```python
# Beta-Header: tool-search-tool-2025-10-19
tools = [
    {
        "name": "selten_genutztes_tool",
        "description": "...",
        "input_schema": {...},
        "defer_loading": True  # Claude lädt nur bei Bedarf via Search
    }
]

antwort = client.beta.messages.create(
    model="claude-opus-4-8",
    max_tokens=4096,
    betas=["tool-search-tool-2025-10-19"],
    tools=tools,
    messages=[...]
)
```


## Inference Geo

Routing in eine bestimmte Geografie erzeugt einen **1,1×-Multiplikator** auf alle Token-Kategorien für Opus 4.6+ und Sonnet 4.6:

```python
antwort = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    inference_geo="EU",  # 1,1× Kostenmultiplikator
    messages=[...]
)
```

Nur verwenden, wenn Data Residency eine Compliance-Anforderung ist — die Kosten summieren sich bei hohem Volumen.


*← [Custom Instructions](./custom-instructions.md) | [Zurück zur deutschen README →](./README.md)*
