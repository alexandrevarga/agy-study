# Antigravity SDK — Quick Reference

---

## Install & Import

```bash
pip install google-antigravity
```

```python
from google.antigravity import Agent, LocalAgentConfig, from_file
from google.antigravity.hooks.policy import deny, allow, ask_user
```

---

## Core API

```python
# Basic usage
config = LocalAgentConfig()
async with Agent(config) as agent:
    response = await agent.chat("prompt")
    text = await response.text()

# With streaming
async for chunk in agent.stream("prompt"):
    print(chunk, end="")

# With structured output (Pydantic)
result: MyModel = await agent.chat("prompt", output_schema=MyModel)

# With multimodal input
response = await agent.chat(["Describe this", from_file("/path/to/image.png")])
```

---

## Safety Policies

```python
from google.antigravity.hooks.policy import deny, allow, ask_user

policies = [
    deny("*"),                              # Block everything by default
    allow("view_file"),                     # Allow specific tools silently
    allow("list_dir"),
    ask_user("run_command", handler=fn),    # Human approval required
    ask_user("write_to_file", handler=fn),
]
config = LocalAgentConfig(policies=policies)
```

| Function | Effect |
|---|---|
| `deny(tool)` | Hard-block, cannot execute |
| `allow(tool)` | Execute silently, no prompt |
| `ask_user(tool, handler=fn)` | Pause, call handler for approval |

Use `"*"` to match all tools.

---

## Lifecycle Hooks

| Category | Blocking | Can modify | Use for |
|---|---|---|---|
| **Inspect** | No | No | Logging, audit, metrics |
| **Decide** | Yes | No | Approval/denial logic |
| **Transform** | Yes | Yes | Sanitize data, error recovery |

**9 lifecycle points:** session_start, pre_turn, post_turn, pre_tool_call, post_tool_call, pre_model_call, post_model_call, step_complete, session_end

```python
from google.antigravity.hooks import Hook

def my_logger(event):
    print(f"Tool: {event.tool_name}")

config = LocalAgentConfig(hooks=[
    Hook(on="pre_tool_call", handler=my_logger, category="inspect")
])
```

---

## Multimodal Support

| Media type | How to pass |
|---|---|
| Image | `from_file("/path/to/image.png")` |
| PDF | `from_file("/path/to/doc.pdf")` |
| Audio | `from_file("/path/to/audio.mp3")` |
| Video | `from_file("/path/to/video.mp4")` |

---

## Key Capabilities

| Capability | API |
|---|---|
| Streaming output | `agent.stream("prompt")` |
| Multimodal input | `from_file()` in message list |
| Structured output | `agent.chat(..., output_schema=PydanticModel)` |
| Token usage | `response.usage.input_tokens` / `.output_tokens` |
| Thinking traces | `response.thinking_traces` |
| Sub-agents | Built into agent via `invoke_subagent` tool |

---

## SDK vs Other Surfaces

| When you need | Use |
|---|---|
| Interactive terminal workflow | CLI |
| Async/scheduled tasks, voice, Projects | 2.0 |
| Full IDE + Tab autocomplete + Browser | IDE |
| Embed agents in your app / CI/CD | **SDK** |
| Structured typed output (Pydantic) | **SDK** |
| Full programmatic policy control | **SDK** |

---

*SDK — June 2026*
