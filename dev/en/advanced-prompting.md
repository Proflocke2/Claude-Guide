# Advanced Prompt Engineering for Claude

> Chapter 1 of the Claude Master Guide — English  
> Covers: XML structure, Chain-of-Thought, Structured Outputs, role prompting, multishot examples.


## 1. The XML Prompt Architecture

Claude was explicitly fine-tuned to recognize XML-style angle-bracket tags as prompt-organizing primitives. There are no canonical "magic" names — but Anthropic's documented conventions are:

| Tag | Position | Purpose |
|---|---|---|
| `<role>` / `<context>` | First | Who Claude is + background it needs |
| `<task>` | After context | The core instruction |
| `<instructions>` | After task | Rules, constraints, preferences |
| `<document>` / `<documents>` | Top of prompt | Long-context source material |
| `<examples>` / `<example>` | Before output spec | Multishot patterns |
| `<thinking>` | CoT scratchpad | Reasoning (separate from answer) |
| `<answer>` | Final block | Extractable output |
| `<output_format>` | Last | Exact structure of the response |

> Anthropic measured it: XML-structured prompts produce **measurably more consistent outputs *(up to 40% hallucination reduction — community-reported, not officially documented)* *(→ [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices))*** than unstructured equivalents.

### Canonical Prompt Template

```xml
<role>
You are a senior data analyst at a Series B SaaS company.
You specialize in churn analysis and cohort modeling.
</role>

<context>
The company has 12,000 MAU. Churn rate is 4.2% MoM.
We use Stripe + Mixpanel. The CFO needs a board-ready explanation.
</context>

<task>
Analyze the attached cohort data and identify the top 3 drivers of churn.
</task>

<instructions>
- Prioritize statistically significant drivers (p < 0.05).
- Do NOT speculate beyond the data provided.
- Use business language, not statistical jargon.
- Flag any data quality issues you observe.
</instructions>

<documents>
  <document index="1">
    <source>cohort_q1_2026.csv</source>
    <document_content>
    [paste data here]
    </document_content>
  </document>
</documents>

<output_format>
Return three sections:
1. **Executive Summary** (2–3 sentences)
2. **Top 3 Churn Drivers** (table with driver, evidence, confidence)
3. **Recommended Actions** (bullet list, prioritized by impact)
</output_format>
```


## 2. Chain-of-Thought (CoT) — Three Levels

Use CoT for tasks a human would need to think through: multi-factor decisions, multi-step math, complex analysis. Skip it for simple lookups.

### Level 1 — Inline Trigger (Minimal)

Append to any prompt. Zero overhead.

```xml
<task>
Calculate the break-even point for the following SaaS pricing change.
Think step by step.
</task>
```

### Level 2 — Guided Reasoning Steps

Outline the specific reasoning path when the domain is well-defined.

```xml
<instructions>
Reason through this in the following order:
1. Identify all cost variables mentioned in the context.
2. Identify all revenue variables.
3. Build the break-even equation explicitly.
4. Solve numerically.
5. State assumptions made.
Only then provide your conclusion.
</instructions>
```

### Level 3 — Structured CoT with XML Tags (Programmatic Extraction)

Best for pipelines where you need to log, evaluate, or branch on reasoning separately from output.

```xml
<instructions>
Use <thinking> tags for your reasoning process.
Use <answer> tags for the final response only.
The system will extract only <answer> content for the user.
</instructions>
```

Response structure you can parse programmatically:

```xml
<thinking>
The user is asking about X. First I need to consider...
Constraint A rules out option 2 because...
Therefore the correct approach is...
</thinking>

<answer>
[Clean, user-facing response here]
</answer>
```

> **Extended thinking models:** For `claude-opus-4-8` with `effort: max`, prefer general instructions over prescriptive steps. Anthropic notes: *"Claude's creativity may exceed your ability to prescribe the optimal thinking process."*


## 3. Structured Outputs — Replacing Prefilling

**Prefilling is deprecated.** Sending `{` as the start of an assistant message (to force JSON) returns a 400 error on current models.

### Option A — `output_config.format` (Guaranteed JSON via schema)

```python
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    output_config={
        "format": {
            "type": "json_schema",
            "json_schema": {
                "name": "churn_analysis",
                "schema": {
                    "type": "object",
                    "properties": {
                        "drivers": {
                            "type": "array",
                            "items": {
                                "type": "object",
                                "properties": {
                                    "name": {"type": "string"},
                                    "confidence": {"type": "number"},
                                    "evidence": {"type": "string"}
                                },
                                "required": ["name", "confidence", "evidence"]
                            }
                        },
                        "executive_summary": {"type": "string"},
                        "data_quality_flags": {
                            "type": "array",
                            "items": {"type": "string"}
                        }
                    },
                    "required": ["drivers", "executive_summary"]
                },
                "strict": True
            }
        }
    },
    messages=[{"role": "user", "content": "Analyze this churn data: [...]"}]
)
```

### Option B — `strict: true` on Tool Definitions

Use the **structured-output-as-tool** pattern when you want schema-guaranteed inputs without a natural tool action:

```python
tools = [
    {
        "name": "return_analysis",
        "description": "Returns the structured analysis result. Call this exactly once with your complete analysis.",
        "input_schema": {
            "type": "object",
            "properties": {
                "drivers": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "name": {"type": "string"},
                            "confidence": {"type": "number", "minimum": 0, "maximum": 1},
                            "evidence": {"type": "string"}
                        },
                        "required": ["name", "confidence", "evidence"]
                    }
                },
                "summary": {"type": "string"}
            },
            "required": ["drivers", "summary"]
        },
        "strict": True  # grammar-constrained sampling — guaranteed schema match
    }
]

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    tools=tools,
    tool_choice={"type": "tool", "name": "return_analysis"},  # force the call
    messages=[{"role": "user", "content": "Analyze this data: [...]"}]
)

# Extract result
tool_use = next(b for b in response.content if b.type == "tool_use")
result = tool_use.input  # guaranteed to match schema
```

> Strict mode limits: Combined ~24-parameter cap across all strict schemas per request. Incompatible with citations and (deprecated) prefilling.


## 4. Role Prompting

Always place role in the **`system` parameter**, not the user message.

```python
system_prompt = """
<role>
You are the General Counsel of a Fortune 500 technology company.
You have 20 years of experience in SaaS licensing, GDPR compliance, and M&A due diligence.
You communicate in precise legal language but always surface the business risk clearly.
</role>

<context>
You are advising the product team on API terms of service questions.
You do not provide final legal advice — you frame risks and recommend escalation thresholds.
</context>
"""
```

Why it works: Anthropic's docs note that setting Claude as *"the General Counsel of a Fortune 500 tech company"* dramatically sharpens legal analysis vs. an unsystematized prompt. The role creates a prior over vocabulary, caution level, and response structure.


## 5. Multishot Examples

**3–5 diverse, relevant, structured examples** is the documented sweet spot. Wrap in `<examples>` tags.

```xml
<examples>
  <example>
    <input>Customer churned after 45 days. Last login: 3 days before cancellation. Support tickets: 0.</input>
    <output>
    Classification: Silent churn
    Primary driver: Activation failure (low engagement pattern)
    Confidence: High
    Recommended action: Retroactive onboarding audit for similar cohort
    </output>
  </example>

  <example>
    <input>Customer churned after 14 months. 3 support tickets in final 30 days. Ticket topics: billing dispute, feature request denied x2.</input>
    <output>
    Classification: Dissatisfaction churn
    Primary driver: Unresolved friction (support escalation pattern)
    Confidence: Medium-High
    Recommended action: Win-back sequence with feature roadmap context
    </output>
  </example>

  <example>
    <input>Customer churned after 6 months. No support tickets. Downloaded data export 2 weeks prior.</input>
    <output>
    Classification: Competitor migration
    Primary driver: Pre-planned exit (data export signal)
    Confidence: Medium
    Recommended action: Competitive positioning review; exit survey outreach
    </output>
  </example>
</examples>
```


## 6. Long-Context Document Handling

For RAG and multi-document tasks:

```xml
<documents>
  <document index="1">
    <source>contract_2024.pdf</source>
    <document_content>[full text]</document_content>
  </document>
  <document index="2">
    <source>contract_2026.pdf</source>
    <document_content>[full text]</document_content>
  </document>
</documents>

<task>
Compare the liability clauses between document 1 and document 2.
First, quote the relevant sections from each document in <quotes> tags.
Then provide your analysis.
</task>
```

> Layout rule: Documents at the **top** of the prompt, query and instructions at the **bottom**. Up to **30% quality improvement** with this ordering at long context *(community-reported — no official Anthropic source)*.

For multi-document RAG, asking Claude to quote relevant sections in `<quotes>` before answering improves grounding and reduces hallucination significantly even at 200K context *(community-reported result — no official Anthropic source)*.


## 7. The Think Tool (Agentic Workflows)

Distinct from extended thinking — the `think` tool gives Claude a mid-loop scratchpad to reflect before calling the next tool.

```python
tools = [
    {
        "name": "think",
        "description": "Use this to reason about what you've gathered so far and decide the next step. Do not use for final answers.",
        "input_schema": {
            "type": "object",
            "properties": {
                "thought": {"type": "string"}
            },
            "required": ["thought"]
        }
    },
    # ... your actual tools
]
```

Per Anthropic's engineering post: a `think` tool contributed to the state-of-the-art SWE-bench score of 0.623 for Claude 3.7 Sonnet, with significant improvements on policy-heavy customer-service tasks when combined with optimized prompting.


*← [Back to English README](./README.md) | Next: [Model Matrix →](./model-matrix.md)*
