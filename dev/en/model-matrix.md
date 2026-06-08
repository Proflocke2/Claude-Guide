# Model Selection Matrix

> Chapter 2 of the Claude Master Guide — English  
> Current as of June 2026. Deprecated IDs retire June 15, 2026 *(→ [Model deprecations](https://platform.claude.com/docs/en/about-claude/model-deprecations))*.


## Current Production Models

| Model ID | Context | Max Output | Batch Output | Price (in / out) |
|---|---|---|---|---|
| `claude-opus-4-8` | 1M tokens | 128K | 300K (beta header) | $5 / $25 per MTok |
| `claude-sonnet-4-6` | 1M tokens | 64K | 300K (beta header) | $3 / $15 per MTok |
| `claude-haiku-4-5-20251001` | 200K tokens | 64K | — | $1 / $5 per MTok |

Batch API: Flat 50% discount *(→ [Batch API](https://platform.claude.com/docs/en/build-with-claude/batch-api))* on all calls; extends max output to 300K on Opus 4.6+/Sonnet 4.6 via `output-300k-2026-03-24` beta header. No quality difference for non-real-time workloads.

Prompt caching multipliers: 5-min writes at 1.25×, 1-hour writes at 2×, cache reads at 0.1× base input. Caching pays off after one read at 5-min TTL or two reads at 1-hour TTL.


## Decision Flowchart

```
Start: What does your task require?
│
├─ Autonomous agents (>10 min), complex multi-file refactors,
│  legal/financial reasoning, long-horizon orchestration
│  └─→  claude-opus-4-8
│       (Only model to clear Anthropic's internal "Super-Agent" benchmark;
│        84% on Online-Mind2Web (83.4% OSWorld-Verified) computer use)
│
├─ Production chat, mid-tier coding, RAG QA, document analysis,
│  frontend design, computer use, most agentic workloads
│  └─→  claude-sonnet-4-6  ← DEFAULT
│       (Near-Opus quality at 60% lower cost; stronger instruction-following
│        than 4.5; default for most production traffic)
│
└─ Classification, extraction, summarization, routing,
   sub-agent fan-out, high-volume parallel execution, free-tier products
   └─→  claude-haiku-4-5-20251001
        (Matches Sonnet 4 on reasoning/coding at fraction of latency;
         first Haiku with extended thinking, computer use, bash, web search;
         73.3% SWE-bench Verified)
```


## Capability Comparison

| Capability | Opus 4.8 | Sonnet 4.6 | Haiku 4.5 |
|---|:---:|:---:|:---:|
| Frontier reasoning | ✅ Best | ✅ Strong | ✅ Good |
| Agentic coding | ✅ Best | ✅ Strong | ⚠️ Sub-agent only |
| Computer use (OSWorld-Verified) | 84% | 72.5% | ✅ Supported |
| Extended thinking | ✅ | ✅ | ✅ (first Haiku with it) |
| Parallel tool calls | ✅ | ✅ | ✅ |
| Structured Outputs | ✅ | ✅ | ✅ |
| Strict tool schemas | ✅ | ✅ | ✅ |
| Batch API (300K output) | ✅ | ✅ | ❌ |
| 1M context window | ✅ | ✅ | ❌ (200K) |
| `effort: max` | ✅ | ❌ | ❌ |


## Routing Strategy by Use Case

### Production API Traffic

```python
def route_model(task: dict) -> str:
    """
    Rule-based model router for cost optimization.
    CloudZero customer data: basic routing drops blended cost 40–60%.
    """
    # Tier 1 — Haiku (classification / extraction / routing)
    if task["type"] in ("classify", "extract", "route", "summarize"):
        return "claude-haiku-4-5-20251001"
    
    # Tier 2 — Sonnet (default production)
    if task["complexity"] < 8 and task["autonomous_minutes"] < 10:
        return "claude-sonnet-4-6"
    
    # Tier 3 — Opus (justified by failure cost)
    if (task["complexity"] >= 8 
        or task["autonomous_minutes"] >= 10
        or task["domain"] in ("legal", "financial", "multi_file_refactor")):
        return "claude-opus-4-8"
    
    return "claude-sonnet-4-6"  # safe default
```

### Effort Levels (Sonnet 4.6 / Opus 4.8)

```python
# effort replaces budget_tokens — now generally available
effort_map = {
    "simple_qa": "low",
    "standard_coding": "medium",
    "complex_analysis": "high",
    "coding_agent": "xhigh",     # recommended for coding/agentic on Opus 4.7+
    "frontier_agent": "max",     # max effort, Opus 4.8
}

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=4096,
    thinking={"type": "adaptive"},       # adaptive, not "enabled"
    output_config={"effort": "high"},    # effort belongs in output_config
    messages=[...]
)
```

> ⚠️ `effort: high` on Sonnet 4.6 may add latency vs. Sonnet 4.5 if you were previously running unconfigured. Benchmark your p95 before rolling out.


## Cost Modeling — Worked Examples

### Example 1: High-Volume Classification Pipeline

```
Task: Classify 1M customer support tickets per month
Complexity: Low (structured input → fixed taxonomy)
Model: claude-haiku-4-5-20251001

Estimated tokens per request: ~800 in + ~50 out
Monthly cost:
  Input:  1,000,000 × 800 / 1,000,000 × $1.00  = $800
  Output: 1,000,000 × 50  / 1,000,000 × $5.00  = $250
  Total: ~$1,050/month

With prompt caching (repeated system prompt, ~500 tokens):
  Cache reads at 0.1× → $800 × 0.1 = $80 input
  New total: ~$330/month  (69% reduction)
```

### Example 2: Autonomous Coding Agent

```
Task: Multi-file refactor, 50 files, ~10 min autonomous execution
Model: claude-opus-4-8

Estimated tokens per session: ~80K in + ~20K out
Daily sessions: 50 developers × 3 sessions = 150
Monthly cost (22 working days):
  Input:  150 × 22 × 80K / 1M × $5.00  = $1,320
  Output: 150 × 22 × 20K / 1M × $25.00 = $1,650
  Total: ~$2,970/month

Note: Anthropic reports average Claude Code cost ~$13/developer/active day,
with 90% of users under $30/day. Verify against actual usage before budgeting.
```

### Example 3: RAG Document Analysis (Batch)

```
Task: Nightly analysis of 10K documents, non-real-time
Model: claude-sonnet-4-6 via Batch API

Batch discount: 50% on all token categories
Standard cost per 1K docs: $3 × avg_input_tokens + $15 × avg_output_tokens
Effective cost: 50% of above
```


## Deprecated Model Reference

| Model ID | Status | Replacement | Old Pricing |
|---|---|---|---|
| `claude-sonnet-4-20250514` | ❌ [Retired June 15, 2026](https://platform.claude.com/docs/en/about-claude/model-deprecations) | `claude-sonnet-4-6` | $3 / $15 |
| `claude-opus-4-20250514` | ❌ Retired June 15, 2026 | `claude-opus-4-8` | $15 / $75 |
| `claude-3-5-haiku-20241022` | ❌ Already retired | `claude-haiku-4-5-20251001` | $0.80–$1 / $4 |

> Opus 4.8 pricing ($5/$25) represents a 67% cut from Opus 4 ($15/$75)** — the single largest cost reduction in the Claude 4.x family.


## Plan Tier by Team Size

| Scenario | Recommended Plan |
|---|---|
| Solo developer, ≤10 daily sessions | Pro ($17–20/mo) |
| Pro session limit warnings 3+ times/week | Max ($100–200/mo) |
| 3+ people needing shared Projects/billing | Team Standard ($20/seat/mo, min 5 seats) |
| Engineering team with heavy Claude Code usage | Team Premium ($100/seat/mo, min 5 seats) |
| Need SCIM, audit logs, HIPAA/BAA, IP allowlisting, >150 seats | Enterprise ($20/seat + API usage, min 20 self-serve) |

> Quick clarification on context windows: "500K context is Enterprise-only" is incorrect. Per Anthropic's support docs, 500K is available **on all paid plans** for Opus 4.6+/Sonnet 4.6 in chat. The Team plan's marketing page showing "200K" is outdated — the dedicated context-window support article is authoritative.


*← [Advanced Prompting](./advanced-prompting.md) | Next: [Projects & Artifacts →](./projects-artifacts.md)*
