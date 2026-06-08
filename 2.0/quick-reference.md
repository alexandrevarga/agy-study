# Antigravity 2.0 — Quick Reference

---

## Key Paths

| What | Path |
|---|---|
| App data | `~/.gemini/antigravity/` |
| MCP config | `~/.gemini/config/mcp_config.json` |
| OAuth tokens | `~/.gemini/antigravity/mcp_oauth_tokens.json` |
| Skills (global) | `~/.gemini/config/skills/` |
| Skills (workspace) | `.agents/skills/` |
| Plugins (global) | `~/.gemini/config/plugins/` |
| Plugins (workspace) | `.agents/plugins/` |
| Rules (global) | `~/.gemini/GEMINI.md` |
| Rules (workspace) | `.agents/rules/` |
| Hooks | `.agents/hooks.json` or `~/.gemini/config/hooks.json` |
| Sidecars | `~/.gemini/config/sidecars/` |
| Shared config | `~/.gemini/config/` |

---

## Settings Hierarchy

```
Global → Project → Conversation
(each level overrides the parent)
Access: ... dropdown → Settings
```

---

## Customizations Summary

| Type | Global path | Workspace path | Format |
|---|---|---|---|
| MCP | `~/.gemini/config/mcp_config.json` | — | JSON |
| Skills | `~/.gemini/config/skills/` | `.agents/skills/` | SKILL.md |
| Rules | `~/.gemini/GEMINI.md` | `.agents/rules/` | Markdown |
| Workflows | `~/.gemini/config/workflows/` | `.agents/workflows/` | Markdown |
| Plugins | `~/.gemini/config/plugins/` | `.agents/plugins/` | plugin.json |
| Hooks | `~/.gemini/config/hooks.json` | `.agents/hooks.json` | JSON |
| Sidecars | `~/.gemini/config/sidecars/` | — | sidecar.json |

---

## Rules Activation Modes

| Mode | Trigger |
|---|---|
| Always On | Every conversation |
| Manual | `@mention` in input |
| Model Decision | Agent evaluates relevance |
| Glob | Files matching pattern (e.g., `*.ts`) |

---

## Permissions

**Precedence:** deny > ask > allow

| Action | Target format |
|---|---|
| `read_file` | Absolute path or dir |
| `write_file` | Absolute path or dir |
| `read_url` | Domain (matches subdomains) |
| `execute_url` | Domain (matches subdomains) |
| `command` | Command prefix |
| `unsandboxed` | Command prefix |
| `mcp` | `server/tool` or `server/*` |

---

## Built-in Subagents

| Agent | Capabilities | How to invoke |
|---|---|---|
| `research` | Read-only (files, web, grep) | `invoke_subagent` |
| `self` | Full capabilities | `invoke_subagent` |
| `browser` | Browser interaction | `/browser` command only |

Max nesting depth: **10 levels**

---

## Artifacts

| Type | When | Commentable | Exclusive |
|---|---|---|---|
| Implementation Plan | Before changes (Planning mode) | ✅ | — |
| Walkthrough | After task completion | ✅ | — |
| Screenshots | Browser capture | ✅ | — |

---

## Exclusive Features (2.0 only)

| Feature | Description |
|---|---|
| Projects model | Named workspaces with scoped settings |
| Worktrees | Multiple git branches without stashing |
| Sidecars | Background processes (auto-restart) |
| Scheduled Tasks | Cron-like recurring agent tasks |
| Voice transcription | Native, no external tool |
| /teamwork-preview | Multi-agent orchestration (Ultra only) |

---

## Models

| Model | Tier | Use case |
|---|---|---|
| Gemini 3.1 Pro | All | Main agent (default) |
| Gemini 3.5 Flash | All | Speed/efficiency |
| Gemini 3.1 Flash | All | Browser agent |
| Claude Sonnet 4.6 | Ultra only | Third-party |
| Claude Opus 4.6 | Ultra only | Third-party |
| GPT-OSS-120b | Ultra only | Third-party |

---

## Plans

| Plan | Refresh | Third-party | Overages |
|---|---|---|---|
| Basic | Weekly | ❌ | ❌ |
| Pro | 5h | ❌ | ✅ |
| Ultra | 5h | ✅ | ✅ |

Tab completions: **Unlimited** on all plans.

---

## Hooks Events

| Event | Fires | Output |
|---|---|---|
| PreToolUse | Before tool | `decision` (allow/deny/ask/force_ask), `reason`, `permissionOverrides` |
| PostToolUse | After tool | `{}` |
| PreInvocation | Before model | `injectSteps` |
| PostInvocation | After tool calls | `injectSteps`, `terminationBehavior` |
| Stop | Loop terminates | `decision` ("continue" to prevent), `reason` |

---

## Version Timeline

| Date | Version | Key milestone |
|---|---|---|
| Nov 2025 | 1.11.2 | Original launch |
| Jan 2026 | 1.14.2 | Skills introduced |
| Feb 2026 | 1.16.5 | Strict Mode rename |
| Mar 2026 | 1.21.6 | Linux Sandboxing |
| Apr 2026 | 1.22.2 | Unified permissions |
| May 19, 2026 | 2.0.1 | **2.0 launch** |
| Jun 3, 2026 | 2.0.11 | Current |

---

*v2.0.11 — June 2026*
