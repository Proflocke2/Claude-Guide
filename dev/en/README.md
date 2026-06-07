# 🧠 Claude Master Guide — English

> Senior-level reference for Claude API, Prompt Engineering, Projects, Tools, and Enterprise deployment.  
> All content reflects the **June 2026** production state of the Claude API.


## 📚 Chapters

| # | Chapter | Description |
|---|---|---|
| 1 | [Advanced Prompt Engineering](./advanced-prompting.md) | XML tags, Chain-of-Thought, Structured Outputs |
| 2 | [Model Selection Matrix](./model-matrix.md) | Opus vs. Sonnet vs. Haiku — cost, latency, capability |
| 3 | [Projects & Artifacts](./projects-artifacts.md) | Knowledge bases, context window optimization, Artifact types |
| 4 | [Custom Instructions & System Prompts](./custom-instructions.md) | Templates, role prompting, persistent configs |
| 5 | [Tools & Connectors](./tools-connectors.md) | Tool use, MCP, Function Calling, built-in tools |


## 🚨 Critical Migration Notice

```
claude-opus-4-20250514   →  claude-opus-4-8
claude-sonnet-4-20250514 →  claude-sonnet-4-6
claude-3-5-haiku-20241022 → claude-haiku-4-5  (already retired)
```

Deadline: June 15, 2026.** The API returns a hard failure after this date — no fallback, no auto-routing. Run a ripgrep across your entire monorepo:

```bash
rg "20250514" --type-list
rg "20250514" .
# Also check:
rg "claude-3-5-haiku" .
rg "budget_tokens" .       # deprecated — use effort: high|medium|low|max
```


## 🔑 API Behavioral Changes in 4.6 / 4.7 / 4.8

| Change | Old behavior | New behavior |
|---|---|---|
| **Prefilling** | Prefill assistant turn with `{` to force JSON | Use Structured Outputs (`output_config.format`) |
| **Sampling params** | `temperature`, `top_p`, `top_k` freely settable | Returns **400 error** on Opus 4.7+ if non-default |
| **Thinking budget** | `budget_tokens` beta header | `effort: high \| medium \| low \| max` (xhigh in Claude Code) |
| **Batch API** | 2× premium for extended output | Flat pricing, 50% discount, 300K output via beta header |
| **Token output** | — | New tokenizer produces up to 35% more tokens for same text |


## 💡 Five Principles Before You Start

1. **XML structure first.** Claude was fine-tuned on XML tags as prompt-organizing primitives. Use them.
2. **`system` is for role + persistent context only.** Task-specific instructions → user turn.
3. **Structured Outputs, not prefilling.** Prefilling returns 400 on current models.
4. **Documents → top. Query → bottom.** Anthropic reports up to 30% quality improvement.
5. **Write tool descriptions like docstrings.** The description is what the model reads to decide *when* to call.


*← [Back to root](../README.md)*
