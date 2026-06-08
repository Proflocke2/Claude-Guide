# Projects & Artifacts

> Chapter 3 of the Claude Master Guide — English  
> Covers: Projects architecture, context window optimization, Artifact types and capabilities.


## Claude Projects

Projects are persistent workspaces that combine: **(1) a knowledge base, (2) custom instructions, and (3) chat history** — all kept in scope across sessions.

### File Limits

| Type | Limit |
|---|---|
| Supported formats | PDF, DOCX, TXT, CSV, HTML, ODT, RTF, EPUB; images up to 30 MB / 8000×8000 px |
| Chat interface (outside Projects) | Max 20 files per single conversation |
| Files API (developer tier) | Up to 5 GB server-side, referenced by ID |
| PDF visual analysis | Full (text + charts + images) under 100 pages; text-only fallback above |

### Context Window in Projects

| Model | Standard context | Chat with Opus 4.6+ / Sonnet 4.6 |
|---|---|---|
| All paid plans | 200K | 500K |
| Claude Code | 1M (credits required on Pro/Team; no gate on usage-based Enterprise) |

> **Common misconception:** "500K context is Enterprise-only" is wrong. Per Anthropic's support docs, 500K is available on **all paid plans** *(→ [Support article](https://support.claude.com/en/articles/8606394))* when chatting with Opus 4.6+/Sonnet 4.6.

### RAG Auto-Trigger

Projects automatically activate retrieval-augmented generation (RAG) when project knowledge approaches the context-window limit, expanding effective capacity by up to **10×** via a `project_knowledge_search` tool.

> ⚠️ **Known bug** (GitHub issue #25759 on anthropics/claude-code): RAG mode appears to activate at **~13 files** regardless of total token count — even at ~2% of stated capacity. If you have many small files, expect RAG-mode retrieval earlier than the docs suggest. Watch for this in agent pipelines.

### When to Use Projects vs. External Storage

| Scenario | Use |
|---|---|
| ≤~30 files, under 30 MB each, paid plan | **Projects** |
| Files routinely exceed 30 MB | External storage + MCP |
| Same knowledge base across multiple AI clients | External storage + MCP |
| Agents needing programmatic file access | External storage + MCP |
| Files you'll reference more than once | **Projects** (cached content doesn't consume message budget on reuse) |

### Project Optimization Checklist

```
✅ Keep project instructions SHORT — reserve task-specific instructions for the chat
✅ Remove unused files (especially in Claude Code — MCP tool definitions are token-heavy)
✅ Toggle extended thinking OFF when not needed
✅ Use Projects for repeated-reference content (caching benefit)
✅ For paid plans with code execution: server-side compaction handles older messages automatically
✅ Split large monolithic documents into smaller topical files for better RAG retrieval
```


## Artifacts

Artifacts are standalone, editable, iteratable content windows that open beside the chat. Anthropic triggers an Artifact when content is *"significant and self-contained, typically over 15 lines"* and something you're likely to want to *"edit, iterate on, or reuse outside the conversation."*

### Artifact Types

| Type | Best for | Notes |
|---|---|---|
| **Documents** | Markdown reports, SOPs, briefs, structured docs | Default for long-form writing |
| **Code** | Scripts in any language, run-ready snippets | Syntax-highlighted, executable in some contexts |
| **Interactive** | Calculators, quizzes, dashboards, games | Most powerful category — full HTML/JS |
| **SVG** | Logos, icons, technical illustrations | Inline rendering |
| **Diagrams** | Flowcharts, sequence diagrams, process maps | Mermaid-based |
| **React Components** | Full single-file UIs with Tailwind, lucide-react, recharts, shadcn/ui | Production-quality UI prototyping |

### React Artifact — Available Libraries

```javascript
// Available imports in React Artifacts (no build step)
import { useState, useEffect, useReducer, useCallback, useMemo } from "react"
import { LineChart, BarChart, XAxis, YAxis, Tooltip, Legend } from "recharts"
import { AlertCircle, Check, ChevronRight } from "lucide-react"  // lucide-react@0.383.0
import _ from 'lodash'
import * as d3 from 'd3'
import * as math from 'mathjs'
import Papa from 'papaparse'      // CSV processing
import * as XLSX from 'xlsx'      // Excel processing
import * as Tone from 'tone'      // Audio
import * as THREE from 'three'    // 3D (r128; no OrbitControls)

// Tailwind: core utility classes only (pre-defined base-stylesheet)
// shadcn/ui: import { Button } from '@/components/ui/button'
// External scripts: https://cdnjs.cloudflare.com only
```

> ⚠️ **No localStorage/sessionStorage** in Artifacts — they are not supported and will fail silently. Use React `useState` for in-session state.

### Artifact Persistent Storage API

For stateful Artifacts (journals, trackers, leaderboards):

```javascript
// Key-value storage persists across sessions
// Personal data (default: shared=false)
await window.storage.set('entries:2026-06-07', JSON.stringify(entry));
const result = await window.storage.get('entries:2026-06-07');
const entry = result ? JSON.parse(result.value) : null;

// Shared data (visible to all users of the Artifact)
await window.storage.set('leaderboard:alice', JSON.stringify(score), true);

// List keys
const keys = await window.storage.list('entries:');  // prefix filter

// Rules:
// - Keys: under 200 chars, no whitespace / slashes / quotes
// - Values: under 5 MB, text/JSON only
// - Rate limited — batch related data into single keys
// - Last-write-wins for concurrent updates
// - Artifact must be published to use shared storage
```

**Key design pattern:** combine data that's updated together into single keys:

```javascript
// ✅ Good — one round-trip
await window.storage.set('user-state', JSON.stringify({ 
  cards, benefits, completion, lastUpdated 
}));

// ❌ Bad — sequential round-trips hit rate limits
await window.storage.set('cards', JSON.stringify(cards));
await window.storage.set('benefits', JSON.stringify(benefits));
await window.storage.set('completion', JSON.stringify(completion));
```

### Embedded Claude API in Artifacts (Pro/Max/Team/Enterprise)

Artifacts can call the Claude API directly — end users authenticate with their own Claude accounts, usage counts against their limits:

```javascript
// Inside an Artifact (React or HTML)
const response = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
        model: "claude-sonnet-4-20250514",  // use current non-deprecated ID
        max_tokens: 1000,
        messages: [{ role: "user", content: userInput }]
    })
});
const data = await response.json();
const text = data.content
    .filter(block => block.type === "text")
    .map(block => block.text)
    .join("\n");
```

### MCP-Connected Artifacts (Pro/Max/Team/Enterprise, web/desktop only)

Artifacts can call MCP tools (Asana, Google Calendar, Slack, custom servers) on first-use approval:

```javascript
// MCP tool call inside an Artifact
const response = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
        model: "claude-sonnet-4-6",
        max_tokens: 1000,
        messages: conversationHistory,
        mcp_servers: [
            {
                type: "url",
                url: "https://mcp.asana.com/sse",
                name: "asana-mcp"
            }
        ]
    })
});

// Process MCP response — results arrive as content blocks
const data = await response.json();
const toolResults = data.content
    .filter(item => item.type === "mcp_tool_result")
    .map(item => item.content?.[0]?.text || "")
    .join("\n");
const textResponse = data.content
    .filter(item => item.type === "text")
    .map(item => item.text)
    .join("\n");
```

> Within Team/Enterprise organizations, intra-org sharing of Artifacts with embedded API calls is free.


*← [Model Matrix](./model-matrix.md) | Next: [Custom Instructions →](./custom-instructions.md)*
