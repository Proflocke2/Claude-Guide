# Tools & Connectors

> Chapter 5 of the Claude Master Guide — English  
> Covers: Tool use architecture, MCP, built-in tools, parallel execution, strict schemas.


## Tool Use — API Architecture

Unlike OpenAI's separate `tool` role, Claude embeds tools directly into the user/assistant message structure:

- **Assistant messages:** contain `text` and `tool_use` blocks
- **User messages:** contain `text` and `tool_result` blocks

One ordering rule that matters: `tool_result` blocks must come **before** any `text` blocks in the user message. Text-before-tool-result returns a 400 error.

```python
# Correct conversation structure
messages = [
    {"role": "user", "content": "What's the weather in Berlin and Paris?"},
    {
        "role": "assistant",
        "content": [
            {"type": "text", "text": "I'll check both cities for you."},
            {"type": "tool_use", "id": "call_1", "name": "get_weather", 
             "input": {"city": "Berlin"}},
            {"type": "tool_use", "id": "call_2", "name": "get_weather", 
             "input": {"city": "Paris"}}
        ]
    },
    {
        "role": "user",
        "content": [
            # tool_result BEFORE text — this is required
            {"type": "tool_result", "tool_use_id": "call_1", 
             "content": [{"type": "text", "text": "18°C, cloudy"}]},
            {"type": "tool_result", "tool_use_id": "call_2", 
             "content": [{"type": "text", "text": "22°C, sunny"}]},
        ]
    }
]
```


## Tool Definition — Best Practices

A tool requires three fields. The **description is the most critical** — it's what the model reads to decide when to call.

```python
tools = [
    {
        "name": "github_list_prs",  # namespace when tools span services
        "description": """
Retrieves open pull requests from a GitHub repository.

USE WHEN:
- User asks about pending PRs, code reviews, or merge queue
- User wants to check the status of open work
- User says "what's waiting for review" or similar

DO NOT USE WHEN:
- User asks about merged or closed PRs (use github_list_closed_prs instead)
- User asks about issues, not PRs
- You already have fresh PR data in context from the last 5 minutes
        """.strip(),
        "input_schema": {
            "type": "object",
            "properties": {
                "owner": {
                    "type": "string",
                    "description": "GitHub organization or username"
                },
                "repo": {
                    "type": "string",
                    "description": "Repository name without owner prefix"
                },
                "state": {
                    "type": "string",
                    "enum": ["open", "closed", "all"],  # enum = fewer hallucinations
                    "description": "PR state filter"
                },
                "limit": {
                    "type": "integer",
                    "minimum": 1,
                    "maximum": 100,
                    "description": "Max results to return (default: 30)"
                }
            },
            "required": ["owner", "repo"]
        }
    }
]
```

### Tool Choice Control

```python
# auto (default) — Claude decides when to call
tool_choice = {"type": "auto"}

# any — force Claude to call at least one tool
tool_choice = {"type": "any"}

# tool — force a specific tool (structured-output-as-tool pattern)
tool_choice = {"type": "tool", "name": "return_analysis"}

# none — disable all tools (saves ~system-prompt tokens for that request)
tool_choice = {"type": "none"}
```

### Strict Schema Mode

`strict: true` enables grammar-constrained sampling — tool inputs are **guaranteed** to match your JSON Schema:

```python
{
    "name": "create_invoice",
    "description": "Creates a new invoice record.",
    "input_schema": {
        "type": "object",
        "properties": {
            "customer_id": {"type": "string"},
            "amount_cents": {"type": "integer", "minimum": 1},
            "currency": {"type": "string", "enum": ["USD", "EUR", "GBP"]},
            "due_date": {"type": "string", "format": "date"}
        },
        "required": ["customer_id", "amount_cents", "currency", "due_date"]
    },
    "strict": True  # guaranteed schema match
}
```

> Strict mode limits: Combined ~24-parameter cap across all strict schemas per request. Incompatible with citations.


## Parallel Tool Execution

Supported on all 4.x models, enabled by default. Sonnet 4.5+ is especially efficient (described by Anthropic benchmark partners as *"running multiple bash commands at once"*).

```python
import anthropic
import asyncio

client = anthropic.Anthropic()

def execute_tool(tool_name: str, tool_input: dict) -> str:
    """Your tool execution logic."""
    match tool_name:
        case "get_weather": return get_weather(**tool_input)
        case "get_stock_price": return get_stock_price(**tool_input)
        case "search_news": return search_news(**tool_input)
        case _: raise ValueError(f"Unknown tool: {tool_name}")

def run_agent_loop(messages: list, tools: list) -> str:
    while True:
        response = client.messages.create(
            model="claude-sonnet-4-6",
            max_tokens=4096,
            tools=tools,
            messages=messages
        )
        
        if response.stop_reason == "end_turn":
            # Extract final text response
            return next(b.text for b in response.content if b.type == "text")
        
        if response.stop_reason == "tool_use":
            # Collect all tool calls (may be parallel)
            tool_calls = [b for b in response.content if b.type == "tool_use"]
            
            # Execute all in parallel
            results = []
            for call in tool_calls:
                result = execute_tool(call.name, call.input)
                results.append({
                    "type": "tool_result",
                    "tool_use_id": call.id,
                    "content": [{"type": "text", "text": str(result)}]
                })
            
            # Append assistant turn + all results
            messages.append({"role": "assistant", "content": response.content})
            messages.append({"role": "user", "content": results})
```


## Built-in Anthropic Tools

These run on Anthropic infrastructure — `type` field only, no custom schema:

| Tool | Description | Pricing |
|---|---|---|
| `web_search` | Real-time web search | Usage-priced per search |
| `web_fetch` | Fetch specific URL content | Included |
| `code_execution` | Sandboxed Python execution | Included |
| `bash` | Shell access | Included |
| `text_editor` | File editing operations | Included |
| `computer_use` | Desktop control (72.5%–84% OSWorld-Verified on Sonnet 4.5+/4.6) | Included |
| `memory` | Persistent agent memory | Included |
| `advisor` | Pair fast executor with smarter advisor mid-generation | Public beta |

```python
# Enable built-in tools
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    tools=[
        {"type": "web_search_20250305", "name": "web_search"},
        {"type": "code_execution_20250522", "name": "code_execution"},
        {"type": "bash_20250124", "name": "bash"}
    ],
    messages=[{"role": "user", "content": "Search for the latest React 19 release notes, then run a quick benchmark."}]
)
```

### Computer Use (Sonnet 4.6 Recommended)

```python
response = client.beta.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=4096,
    tools=[
        {
            "type": "computer_20250124",
            "name": "computer",
            "display_width_px": 1280,
            "display_height_px": 800
        }
    ],
    messages=[{
        "role": "user",
        "content": "Open the browser, navigate to our analytics dashboard, and take a screenshot of the revenue chart."
    }]
)
```


## Programmatic Tool Execution (Code Execution Container)

Instead of round-tripping through the model for each tool invocation, Claude writes Python that calls your tools as functions inside a sandboxed container:

- **20–40% token savings** for requests with 10–49 tools *(per Anthropic's programmatic tool execution docs)*
- Tool results from programmatic invocations don't count against context

```python
# Register your tools as Python functions in the code execution context
tools = [
    {
        "type": "code_execution_20250522",
        "name": "code_execution"
    }
]

system = """
You have access to the following Python functions in your execution environment:

```python
def get_customer(customer_id: str) -> dict: ...
def list_invoices(customer_id: str, status: str = "open") -> list[dict]: ...
def create_refund(invoice_id: str, amount_cents: int) -> dict: ...
```

Call these functions directly in code blocks rather than using separate tool calls.
"""
```


## MCP (Model Context Protocol)

*(→ [Tools overview](https://platform.claude.com/docs/en/build-with-claude/tools/overview))*

Open standard for connecting any MCP-compatible client (Claude.ai, Claude Code, Claude Desktop, VS Code, third-party agents) to any MCP server via JSON-RPC 2.0.

Three transports: `stdio` (local subprocess), `SSE`, `Streamable HTTP`

### Configuration (`.mcp.json`)

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
    "custom-internal": {
      "url": "https://your-mcp-server.internal/sse",
      "transport": "sse"
    }
  }
}
```

### MCP Security Checklist

```
✅ Use ${ENV_VAR} references in .mcp.json — never hardcode secrets
✅ Claude Code v2.1+ supports per-tool allow/deny lists — use them
✅ Prefer @modelcontextprotocol/* and trusted community packages
✅ stdio servers run with your user's file-system permissions — scope carefully
✅ Let Claude Code select tools by intent (description is the ranking signal)
✅ Default output cap: 25,000 tokens per MCP tool call (warning at 10,000)
✅ Tune with MAX_MCP_OUTPUT_TOKENS or per-tool anthropic/maxResultSizeChars
✅ Disable unused servers to keep context lean
```

### Output Size Configuration

```json
{
  "mcpServers": {
    "my-server": {
      "url": "https://my-mcp-server.com/sse",
      "anthropic": {
        "maxResultSizeChars": 50000
      }
    }
  }
}
```

### Processing MCP Responses (Artifact / API Context)

```javascript
async function callWithMCP(userMessage, mcpServers) {
    const response = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
            model: "claude-sonnet-4-6",
            max_tokens: 1000,
            messages: [{ role: "user", content: userMessage }],
            mcp_servers: mcpServers
        })
    });
    
    const data = await response.json();
    
    // Extract by type — never by position
    const toolResults = data.content
        .filter(item => item.type === "mcp_tool_result")
        .map(item => {
            try {
                return JSON.parse(item.content?.[0]?.text || "{}");
            } catch {
                return item.content?.[0]?.text || "";
            }
        });
    
    const textResponse = data.content
        .filter(item => item.type === "text")
        .map(item => item.text)
        .join("\n");
    
    const toolCalls = data.content
        .filter(item => item.type === "mcp_tool_use")
        .map(item => ({ name: item.name, input: item.input }));
    
    return { textResponse, toolResults, toolCalls };
}
```

### Lazy Tool Loading (Agents with 100+ Tools)

```python
# Beta header: tool-search-tool-2025-10-19
# Mark tools as defer_loading to keep context clean
tools = [
    {
        "name": "rarely_used_tool",
        "description": "...",
        "input_schema": {...},
        "defer_loading": True  # Claude loads only when needed via search
    }
]

response = client.beta.messages.create(
    model="claude-opus-4-8",
    max_tokens=4096,
    betas=["tool-search-tool-2025-10-19"],
    tools=tools,
    messages=[...]
)
```


## Inference Geo

Routing to a specific geography incurs a **1.1× multiplier** on all token categories for Opus 4.6+ and Sonnet 4.6:

```python
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    inference_geo="EU",  # 1.1× cost multiplier
    messages=[...]
)
```

Only use when data residency is a compliance requirement — the cost adds up at scale.


*← [Custom Instructions](./custom-instructions.md) | [Back to English README →](./README.md)*
