# Antigravity 2.0 — Mastering Guide

> Compiled from official documentation (21 pages) + changelog (v1.11.2 → v2.0.11).

---

## 1. What is Antigravity 2.0

A **standalone desktop application** — not a code editor. Antigravity 2.0 is the agent-first platform for autonomous development workflows: project management, async task execution, voice, scheduled tasks, and multi-agent coordination.

**Key differentiator from CLI and IDE:**
- **Projects model** with hierarchical scoped settings
- **Worktrees** support (checkout multiple branches simultaneously)
- **Sidecars** (background processes managed by Antigravity)
- **Scheduled Tasks** (cron-like recurring agent work)
- **Voice transcription** (native, no external tool)
- Visual Artifact review pane with browser recording playback
- `/teamwork-preview` (Ultra plan, multi-agent orchestration)

**Platform support:**
- macOS: 12+ (Monterey), Apple Silicon + x86
- Windows: 10 (64-bit)
- Linux: glibc >= 2.28, glibcxx >= 3.4.25 (Fedora 36+, Ubuntu 20+, Debian 10+, RHEL 8+)

---

## 2. Installation & Setup

Download from `antigravity.google/download`. App auto-updates by default. Update mode configurable to manual or none in Settings.

---

## 3. Projects Model

### What is a Project
A **Project** is a named workspace that maps to one or more local directories. Projects are the primary organizational unit in 2.0.

**Project types:**
- **Local**: Maps to a directory on your machine
- **Worktree**: Multiple git worktrees under one project

### Worktrees
- Create and switch between worktrees directly in the app
- Each worktree = independent branch, isolated file state
- Supported natively; not available in CLI or SDK

### Project settings
Each project has its own scoped settings that override global defaults, without affecting other projects.

---

## 4. Settings Hierarchy

```
Global Settings
    └── Project Settings (overrides Global)
            └── Conversation Settings (overrides Project)
```

**Access:** `...` dropdown → Settings

**Resetting:** Each level can be reset independently to its parent's defaults.

---

## 5. Customizations

### 5.1 MCP (Model Context Protocol)

**Canonical path:** `~/.gemini/config/mcp_config.json` (shared with CLI and IDE)

Access via: `...` → Customizations → MCP → "Manage MCP Servers" → "View raw config"

OAuth tokens stored at: `~/.gemini/antigravity/mcp_oauth_tokens.json`

Same config schema as CLI (see CLI guide §7 for full field reference).

**MCP Store:** Browse and install 35+ integrations (Firebase, GitHub, Linear, Supabase, Notion, etc.) via built-in store.

### 5.2 Skills

**Paths:**

| Scope | Path |
|---|---|
| Global | `~/.gemini/config/skills/<skill-folder>/` |
| Workspace | `.agents/skills/<skill-folder>/` |

Skills follow progressive disclosure — agent sees name+description at conversation start, reads full SKILL.md only when relevant.

**SKILL.md frontmatter:**
```yaml
---
name: skill-name
description: What it does and when. Write in third person.
---
```

### 5.3 Rules

**File size limit:** 12,000 characters each

**Paths:**

| Scope | Path |
|---|---|
| Global | `~/.gemini/GEMINI.md` |
| Workspace | `.agents/rules/<rule-name>.md` |

**4 activation modes:**

| Mode | Trigger |
|---|---|
| Always On | Every conversation |
| Manual | `@mention` in input box |
| Model Decision | Agent evaluates relevance |
| Glob | Files matching pattern in context (e.g., `*.ts`) |

**`@` file references:** Use `@filename` inside rules files to compose from other files.

### 5.4 Workflows

A structured sequence of steps for repetitive multi-step tasks.

**Invocation:** `/workflow-name` slash command  
**Nesting:** Workflows can call other workflows  
**File size limit:** 12,000 characters each  
**Agent-generated:** Ask agent to generate a workflow from conversation history

**Running in context:** `@workflows <workflow_name>` in chat (e.g., during Firebase migration)

### 5.5 Plugins

Namespaced bundles combining skills + rules + MCP servers + hooks.

**Global path:** `~/.gemini/config/plugins/`  
**Workspace path:** `.agents/plugins/` or `_agents/plugins/`

**Structure:**
```
plugins/<plugin-name>/
├── plugin.json           # Required
├── mcp_config.json       # Optional
├── hooks.json            # Optional
├── skills/               # Optional
└── rules/                # Optional
```

**Build with Google:** Bundled Google-authored plugins available in Customizations UI. See §9.

### 5.6 Hooks

**Config:** `hooks.json` in `.agents/` or `~/.gemini/config/`

**5 events:** PreToolUse, PostToolUse, PreInvocation, PostInvocation, Stop

(Full I/O contract identical to CLI — see CLI guide §10)

### 5.7 Sidecars

**Exclusive to 2.0** (not in CLI or IDE).

Background processes that Antigravity manages — auto-starts and auto-restarts if they crash.

**Use cases:** Persistent scripts, scheduled recurring tasks, event-reactive processes.

**Discovery:** Antigravity scans for `sidecar.json` files.

**Enable:** `~/.gemini/config/config.json` → `sidecars` section

**`sidecar.json` structure:**
```json
{
  "name": "my-sidecar",
  "command": "/path/to/script.sh",
  "args": ["--flag"],
  "env": { "MY_VAR": "value" },
  "restart": true
}
```

**`agentapi` CLI:** Sidecars can call back into the Antigravity agent via the `agentapi` command-line tool.

---

## 6. Agent Capabilities

### 6.1 Permissions

Same engine as CLI:
- 3 lists: deny > ask > allow
- Actions: read_file, write_file, read_url, execute_url, command, unsandboxed, mcp
- Permissions scoped to Global or Project level

### 6.2 Subagents

**Built-in agents:**

| Agent | Capabilities | Invocation |
|---|---|---|
| `research` | Read-only | `invoke_subagent` |
| `self` | Full capabilities | `invoke_subagent` |
| `browser` | Browser interaction | `/browser` only |

- Subagent states: Running → Idle → Killed
- Re-awaken idle agents via any agent's `send_message`
- Max nesting: **10 levels**
- Permissions from subagents bubble up to parent UI

**Custom subagents:** `define_subagent` tool with configurable toolsets.

### 6.3 /teamwork-preview

**Plan only — Ultra ($200/mo)**

Multi-agent framework: define a high-level goal, platform assembles a team of agents. Includes:
- Automatic task decomposition
- Error recovery and auto-retries
- Inter-agent coordination
- Artifact-based delegation

9-step workflow: Elicit → Plan → Delegate → Execute → Review → Iterate → Finalize

---

## 7. Artifacts

Structured deliverables for async human-agent collaboration. No need to monitor every tool call — review at key milestones.

### Artifact types

| Type | When created | Notes |
|---|---|---|
| **Implementation Plan** | Before making changes (Planning mode) | Commentable, Proceed button, Reviewable |
| **Walkthrough** | After task completion | Summary of all changes, may include screenshots |
| **Screenshots** | Browser subagent captures | Commentable for feedback |

### Artifact review policy

**Planning Mode:** Agent creates plan → you review → Proceed → agent executes  
**Fast Mode:** Agent acts immediately, no plan review

### Steering artifacts
- Comment on any artifact inline to redirect the agent
- Click "Review" button to submit feedback instead of Proceeding
- Agent will iterate on the plan or execute based on your feedback

### Artifacts across surfaces

| Surface | Access |
|---|---|
| 2.0 | Visual sidebar + review pane |
| CLI | Keyboard-driven review panel (`ctrl+r`) |
| IDE | Agent panel + Review Changes pane |

---

## 8. Exclusive Features

### Voice Transcription
- Native integration (no external tool)
- Transcribes your voice directly into the input box

### Scheduled Tasks
- Create recurring agent tasks (cron-like)
- Agent runs automatically at specified times
- Manages background execution independently

### /browser
- Invokes Browser subagent
- Opens and actuates local Chrome
- Captures screenshots and recordings as artifacts

### Worktrees
- Create multiple git worktrees per project
- Switch between branches without stashing
- Only surface with native worktree support (confirmed in FAQ)

### Sidecars
- Background process management
- Auto-restart on crash
- `agentapi` for sidecar-to-agent communication

---

## 9. Build with Google

Bundled plugins and integrations from Google teams. Access via Customizations → Plugins.

**Available integrations (partial list):**

| Category | Integrations |
|---|---|
| Google Cloud | Cloud MCP Server, BigQuery, AlloyDB, Cloud SQL, Spanner, Dataplex |
| Firebase | Firebase Plugin, Firebase Studio migration |
| AI/ML | Gemini API SDK, Kaggle |
| Databases | Neon, Supabase, MongoDB, Redis, Pinecone |
| Dev Tools | GitHub, Linear, SonarQube, Postman, Prisma |
| Frontend | Figma Dev Mode, Locofy |
| Deployment | Netlify, Heroku |
| Other | Notion, Stripe, PayPal, Perplexity |

---

## 10. Models & Plans

### Available models

| Model | Default for | Tier |
|---|---|---|
| Gemini 3.1 Pro | Agent (main) | All plans |
| Gemini 3.5 Flash | Speed/efficiency | All plans |
| Gemini 3.1 Flash | Browser agent | All plans |
| Claude Sonnet 4.6 | Third-party | Ultra only |
| Claude Opus 4.6 | Third-party | Ultra only |
| GPT-OSS-120b | Third-party | Ultra only |

### Plans

| Plan | Quota refresh | Third-party models | Overages |
|---|---|---|---|
| Basic | Weekly | ❌ | ❌ |
| Google AI Pro | Every 5h | ❌ | ✅ (G1 credits) |
| Google AI Ultra | Every 5h | ✅ | ✅ (G1 credits) |

**Tab completions:** Unlimited on all plans.

---

## 11. Changelog Timeline

| Date | Version | Milestone |
|---|---|---|
| Nov 18, 2025 | 1.11.2 | **Original Antigravity launch** |
| Dec 4, 2025 | 1.11.14 | Google One integration (Pro/Ultra rates) |
| Dec 8, 2025 | 1.11.17 | Secure Mode introduced |
| Dec 17, 2025 | 1.12.4 | Gemini 3 Flash + native audio |
| Jan 13, 2026 | 1.14.2 | Agent Skills introduced |
| Jan 23, 2026 | 1.15.6 | Terminal Sandboxing (macOS) |
| Feb 3, 2026 | 1.16.5 | "Secure Mode" renamed "Strict Mode" |
| Feb 19, 2026 | 1.18.3 | Gemini 3.1 Pro, artifact download, input history |
| Mar 9, 2026 | 1.20.5 | AGENTS.md support, auto-continue default |
| Mar 25, 2026 | 1.21.6 | Linux Sandboxing, MCP auth improvements |
| Apr 7, 2026 | 1.22.2 | Unified permissions system |
| May 19, 2026 | 2.0.1 | **Antigravity 2.0 launch** |
| May 21, 2026 | 2.0.3 | Migration flow from 1.0 (import customizations) |
| Jun 2, 2026 | 2.0.4 | Enterprise auth fix |
| Jun 3, 2026 | 2.0.11 | **Current** — antivirus + Open IDE fixes |

---

*Last updated: June 2026 — Antigravity 2.0 v2.0.11*
