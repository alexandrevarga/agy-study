# Antigravity — Surface Comparison

> Decision guide: which surface to use for each scenario.

---

## TL;DR Decision Table

| Scenario | Best surface |
|---|---|
| Terminal workflow, SSH, low overhead | **CLI** |
| Async tasks while you're away | **2.0** |
| Voice input to agent | **2.0** |
| Scheduled recurring agent tasks | **2.0** |
| Managing multiple projects with scoped settings | **2.0** |
| Multiple git branches simultaneously (worktrees) | **2.0** |
| Writing code with AI autocomplete | **IDE** |
| UI testing and browser automation | **IDE** |
| Firebase Studio migration | **IDE** |
| Embedding agents in a Python app | **SDK** |
| CI/CD pipeline automation | **SDK** |
| Typed/validated structured output | **SDK** |
| Multi-agent orchestration from code | **SDK** |

---

## Surface Overview

| Dimension | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| **Type** | TUI (terminal) | Desktop app | Code editor | Python library |
| **Launched** | Separate product | May 2026 | Nov 2025 (1.x), May 2026 (2.x) | — |
| **Current version** | 1.0.6 | 2.0.11 | 2.0.4 | Latest pip |
| **Install** | Binary | App download | App download | `pip install` |
| **Linux support** | ✅ | ✅ Fedora 36+ | ✅ Fedora 36+ | ✅ |

---

## AI Modalities

| Modality | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| Agent (autonomous tasks) | ✅ | ✅ | ✅ | ✅ |
| Tab (autocomplete) | ❌ | ❌ | ✅ unlimited | ❌ |
| Voice transcription | ❌ | ✅ | ❌ | ❌ |
| Browser agent | ✅ via `/browser` | ✅ via `/browser` | ✅ native | ❌ |

---

## Execution Modes

| Mode | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| Planning (review before execute) | ✅ `/planning` | ✅ | ✅ | via hooks |
| Fast (immediate execution) | ✅ `/fast` | ✅ | ✅ | default |
| Headless/non-interactive | ✅ `-p` flag | ❌ | ❌ | ✅ native |
| Scheduled (cron) | ❌ | ✅ | ❌ | via hooks |
| Async (agent works while you're away) | limited | ✅ | limited | ✅ |

---

## Customizations

| Customization | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| MCP Servers | ✅ | ✅ | ✅ | ✅ |
| Skills (SKILL.md) | ✅ | ✅ | ✅ | ✅ |
| Rules (GEMINI.md/.agents/rules/) | ✅ | ✅ | ✅ | via system prompt |
| Workflows (/workflow-name) | ✅ | ✅ | ✅ | — |
| Plugins (plugin.json bundles) | ✅ | ✅ | ✅ | — |
| Hooks (hooks.json) | ✅ | ✅ | ✅ | ✅ (native API) |
| Sidecars (background processes) | ❌ | ✅ | ❌ | — |
| Custom Python tools | ❌ | ❌ | ❌ | ✅ |

---

## Security Features

| Feature | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| deny/ask/allow permissions | ✅ | ✅ | ✅ | ✅ (policy API) |
| Terminal Sandboxing (nsjail/sandbox-exec) | ✅ | ✅ | ✅ | — |
| Strict Mode | ❌ | ❌ | ✅ | — |
| Browser URL Allowlist/Denylist | via MCP | ✅ | ✅ | — |
| Workspace isolation | ✅ | ✅ | ✅ (Strict Mode) | configurable |

---

## Artifacts

| Artifact type | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| Implementation Plan | ✅ | ✅ | ✅ | ✅ |
| Walkthrough | ✅ | ✅ | ✅ | ✅ |
| Screenshots | via browser | ✅ | ✅ | — |
| Browser Recordings | ❌ | ❌ | ✅ **exclusive** | ❌ |
| Code Diffs (Review Changes) | ✅ `/diff` | ✅ | ✅ native pane | — |

---

## Models

| Model | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| Gemini 3.1 Pro | ✅ | ✅ | ✅ | ✅ |
| Gemini 3.5 Flash | ✅ | ✅ | ✅ | ✅ |
| Claude Sonnet 4.6 | Ultra only | Ultra only | Ultra only | Ultra only |
| Claude Opus 4.6 | Ultra only | Ultra only | Ultra only | Ultra only |
| GPT-OSS-120b | Ultra only | Ultra only | Ultra only | Ultra only |

---

## Subagents

| Feature | CLI | 2.0 | IDE | SDK |
|---|---|---|---|---|
| invoke_subagent | ✅ | ✅ | ✅ | ✅ |
| define_subagent | ✅ | ✅ | ✅ | ✅ |
| research built-in | ✅ | ✅ | ✅ | ✅ |
| browser built-in | ✅ | ✅ | ✅ | — |
| self built-in | ✅ | ✅ | ✅ | ✅ |
| Max nesting depth | 10 | 10 | 10 | 10 |
| /teamwork-preview | ✅ Ultra | ✅ Ultra | ✅ Ultra | — |

---

## Key Paths (Per Surface)

| Path | CLI | 2.0 | IDE |
|---|---|---|---|
| App data | `~/.gemini/antigravity-cli/` | `~/.gemini/antigravity/` | `~/.gemini/antigravity/` |
| **MCP config** | `~/.gemini/config/mcp_config.json` | `~/.gemini/config/mcp_config.json` | `~/.gemini/config/mcp_config.json` |
| **Skills (global)** | `~/.gemini/config/skills/` | `~/.gemini/config/skills/` | `~/.gemini/antigravity/skills/` ⚠️ |
| **Plugins (global)** | `~/.gemini/config/plugins/` | `~/.gemini/config/plugins/` | `~/.gemini/config/plugins/` |
| Settings | `~/.gemini/antigravity-cli/settings.json` | App Settings UI | App Settings UI |
| Rules (global) | `~/.gemini/GEMINI.md` | `~/.gemini/GEMINI.md` | `~/.gemini/GEMINI.md` |

> ⚠️ **IDE global skills path is different** from CLI and 2.0. This is a known divergence.

**Shared across all surfaces:** `~/.gemini/config/` — MCP, plugins, sidecars (2.0), global config.

---

## When Each Surface Shines

### Use CLI when:
- You live in the terminal
- You're on SSH or remote machines
- You want lightweight, fast startup
- You need `-p` headless output for scripting
- You use Tmux/screen sessions

### Use 2.0 when:
- You manage multiple projects
- You want agent to work while you step away
- You need voice input
- You want scheduled recurring tasks
- You use git worktrees extensively

### Use IDE when:
- You want Tab autocomplete while writing code
- You're doing UI development and want browser testing
- You're migrating from Firebase Studio
- You want browser recordings of what the agent built
- You need Strict Mode for security-sensitive work

### Use SDK when:
- You're building applications that need embedded agents
- You need typed/structured output (Pydantic)
- You're integrating agents into CI/CD pipelines
- You want full programmatic control over safety policies
- You need streaming output in your application

---

*June 2026 — based on CLI v1.0.6, 2.0 v2.0.11, IDE v2.0.4, SDK latest*
