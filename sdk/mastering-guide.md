# Antigravity SDK — Mastering Guide

> Compiled from the official SDK overview + Quick Start documentation.

---

## 1. What is the SDK

The Antigravity SDK is a **programmatic Python framework** for building, testing, and running autonomous AI agents. It extends the **same core agent harness** that powers the Antigravity CLI and Antigravity 2.0.

**Key value proposition:** Decouples your agent's logic from where it runs. You define *what* the agent does; the SDK handles *how and where* it executes.

**Use the SDK when:**
- You want to embed agents into your own Python applications
- You need programmatic control over agent behavior
- You want to automate agent workflows in CI/CD pipelines
- You need structured output (typed/validated data) from agents
- You want to build multi-agent systems from code

**Use other surfaces instead when:**
- You want interactive development → **CLI** or **IDE**
- You want async/scheduled tasks without code → **2.0**
- You want integrated editing with Tab → **IDE**

---

## 2. Installation

```bash
pip install google-antigravity
```

---

## 3. Quick Start

### Hello World

A functional agent that interacts with your local environment in under 15 lines:

```python
import asyncio
from google.antigravity import Agent, LocalAgentConfig

async def main():
    config = LocalAgentConfig()
    async with Agent(config) as agent:
        response = await agent.chat("What files are in the current directory?")
        print(await response.text())

if __name__ == "__main__":
    asyncio.run(main())
```

**Key classes:**
- `Agent`: The main agent class. Use as an async context manager.
- `LocalAgentConfig`: Configuration for local execution.
- `response.text()`: Awaitable that returns the agent's text output.

---

## 4. Core Pillar 1 — Governed Extensibility (Tools)

Every agent starts with a built-in toolset and can be extended with four types of tools.

### 4.1 Built-in Tools

Included by default, no configuration needed:
- File I/O (read, write, edit)
- Code editing
- Shell execution
- Directory search

### 4.2 Custom Python Functions

Register any Python callable as an agent tool:

```python
def get_weather(city: str) -> str:
    """Returns the current weather for a city."""
    return f"Weather in {city}: sunny, 22°C"

config = LocalAgentConfig(tools=[get_weather])
```

The SDK uses the function's docstring as the tool description for the agent.

### 4.3 MCP Servers

Connect any Model Context Protocol server using stdio, SSE, or HTTP transport:

```python
from google.antigravity.mcp import MCPServerConfig

mcp_config = MCPServerConfig(
    command="/path/to/mcp-server",
    args=["--transport", "stdio"]
)
config = LocalAgentConfig(mcp_servers=[mcp_config])
```

### 4.4 Agent Skills

Load reusable packages of instructions and tools (same SKILL.md format as CLI/2.0/IDE):

```python
from google.antigravity.skills import SkillConfig

skill = SkillConfig(path="/path/to/my-skill/")
config = LocalAgentConfig(skills=[skill])
```

---

## 5. Core Pillar 2 — Declarative Safety Policies

Configure what the agent can and cannot do using a "deny by default" policy system.

### Policy functions

```python
from google.antigravity.hooks.policy import deny, allow, ask_user

policies = [
    deny("*"),                                          # Block ALL tools by default
    allow("view_file"),                                 # Allow reading files silently
    allow("list_dir"),                                  # Allow directory listing
    ask_user("run_command", handler=my_approval_fn),    # Human approval for shell
    ask_user("write_to_file", handler=my_approval_fn),  # Human approval for writes
]

config = LocalAgentConfig(policies=policies)
```

### Policy functions

| Function | Effect |
|---|---|
| `deny(tool_name)` | Hard-block the tool. Agent cannot execute it. |
| `allow(tool_name)` | Allow silently. No user prompt. |
| `ask_user(tool_name, handler=fn)` | Pause and call `handler` for human approval. |

**Target:** Use `"*"` to match all tools. Use exact tool names for specific tools.

**Pattern:** Start with `deny("*")` then selectively `allow()` what you need.

---

## 6. Core Pillar 3 — Lifecycle Hooks

Granular control over agent execution at 9 concrete lifecycle points.

### Hook categories

| Category | Blocking | Can modify | Use for |
|---|---|---|---|
| **Inspect** | No (non-blocking) | No | Logging, audit trails, metrics, tracing |
| **Decide** | Yes | No | Custom approval/denial logic, policies |
| **Transform** | Yes | Yes | Sanitize data in transit, recover from errors |

### Lifecycle points

1. Session start
2. Pre-turn
3. Post-turn
4. Pre-tool-call
5. Post-tool-call
6. Pre-model-call
7. Post-model-call
8. Step complete
9. Session end

### Hook example

```python
from google.antigravity.hooks import Hook

def log_tool_calls(event):
    """Inspect hook: logs all tool calls without blocking."""
    print(f"Tool called: {event.tool_name} with args: {event.args}")

def safety_gate(event):
    """Decide hook: blocks dangerous shell commands."""
    if event.tool_name == "run_command":
        cmd = event.args.get("CommandLine", "")
        if "rm -rf" in cmd:
            return event.deny(reason="Destructive command blocked by policy")
    return event.allow()

config = LocalAgentConfig(hooks=[
    Hook(on="pre_tool_call", handler=log_tool_calls, category="inspect"),
    Hook(on="pre_tool_call", handler=safety_gate, category="decide"),
])
```

---

## 7. Key Capabilities

### 7.1 Streaming

Access live model reasoning and output chunks as they are generated:

```python
async with Agent(config) as agent:
    async for chunk in agent.stream("Analyze this codebase"):
        print(chunk, end="", flush=True)
```

### 7.2 Multimodal Input

Pass images, PDFs, audio, and video natively using `from_file()`:

```python
from google.antigravity import from_file

async with Agent(config) as agent:
    response = await agent.chat([
        "What does this diagram show?",
        from_file("/path/to/architecture.png"),
    ])
```

**Supported:** Images, PDFs, audio files, video files

### 7.3 Sub-agents

Spawn child agents with independent tools and contexts to build multi-agent systems:

```python
async with Agent(config) as orchestrator:
    # Orchestrator delegates to specialized sub-agents
    result = await orchestrator.chat(
        "Spawn a research agent to find Python best practices, "
        "then a coding agent to apply them to this file."
    )
```

### 7.4 Structured Output (Pydantic)

Define schemas using Pydantic models to return validated, typed data directly:

```python
from pydantic import BaseModel
from typing import List

class CodeReview(BaseModel):
    issues: List[str]
    severity: str
    recommended_fixes: List[str]

async with Agent(config) as agent:
    review: CodeReview = await agent.chat(
        "Review this code for issues",
        output_schema=CodeReview
    )
    print(review.severity)  # Typed access
```

### 7.5 Human-in-the-Loop

Pause agent execution to ask structured questions and branch based on user input:

```python
from google.antigravity.interaction import Question, Choice

async with Agent(config) as agent:
    response = await agent.chat("Deploy to production?")
    # Agent can pause and call ask_question tool to get human input
    # Your handler receives the question and provides the answer
```

### 7.6 Observability

Track per-turn and cumulative token usage, and access thinking traces:

```python
async with Agent(config) as agent:
    response = await agent.chat("Complex task...")
    
    # Token usage
    print(response.usage.input_tokens)
    print(response.usage.output_tokens)
    print(response.usage.total_tokens)
    
    # Thinking traces
    for thought in response.thinking_traces:
        print(thought)
```

---

## 8. SDK vs Other Surfaces

| Dimension | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| **Primary interface** | Terminal TUI | Desktop app | Code editor | Python API |
| **Best for** | Developer workflow | Async/scheduled tasks | Integrated editing | Custom apps |
| **Agent control** | Settings/commands | Settings/UI | Settings/UI | Full programmatic |
| **Safety policies** | Permissions system | Permissions system | Permissions system | Declarative policies |
| **Custom tools** | MCP + Skills | MCP + Skills | MCP + Skills | Python functions + MCP |
| **Structured output** | ❌ | ❌ | ❌ | ✅ (Pydantic) |
| **CI/CD integration** | `-p` headless mode | ❌ | ❌ | ✅ (native) |
| **Multimodal** | Limited | ✅ | ✅ | ✅ (`from_file()`) |

---

## 9. Resources

- **Package:** `pip install google-antigravity`
- **GitHub:** Referenced in documentation (URL not published in official docs — check `github.com/google/antigravity`)
- **SDK Skill:** An "Antigravity SDK Skill" is available within Antigravity 2.0 to use the SDK more easily from within the app

---

*Last updated: June 2026*
