# Antigravity IDE — Mastering Guide

> Compiled from 26 pages of official documentation + changelog (v1.11.2 → v2.0.4 IDE).

---

## 1. What is Antigravity IDE

A **full AI-powered IDE** with two distinct AI modalities working in parallel:

- **Agent**: The primary AI. Executes complex, autonomous tasks across editor, terminal, and browser. Creates artifacts, plans, walkthroughs.
- **Tab**: A powerful autocomplete helper operating **exclusively within the text editor**. Does NOT consume quota.

**Core surfaces:**
- **Editor**: VS Code-based, file editing, terminal integration
- **Browser**: Agent-actuated local Chrome with isolated profile

**Platform requirements:**
- macOS: 12+ (Monterey), Apple Silicon (no x86 support)
- Windows: 10 (64-bit)
- Linux: glibc >= 2.28, glibcxx >= 3.4.25 (Fedora 36+, Ubuntu 20+)

---

## 2. Installation

Download from `antigravity.google/download`. App prompts on available updates automatically.

---

## 3. Core Surfaces

### 3.1 Editor

A fully-functional VS Code-based code editor with:
- Full file system access
- Integrated terminal
- Agent Side Panel (right side)
- Review Changes pane

### 3.2 Agent Side Panel

Located on the **right side** of the editor.

**Capabilities:**
- Create new conversations
- Attach images
- Switch agent modes (Planning / Fast)
- Select between models

**Bottom toolbar** (above input box) shows in real-time:
- Open file changes
- Running terminal processes
- Active artifacts

### 3.3 Browser

An embedded local Chrome browser with security isolation. See §6 for full details.

---

## 4. Tab Features

Tab = three distinct features that all share the `Tab` key for acceptance.

### 4.1 Supercomplete

File-wide intelligent code suggestions — not just the current cursor position.

- Can modify code throughout the entire document simultaneously
- Handles tasks like renaming variables across all usages, updating multiple function definitions
- **Accept:** `Tab`

### 4.2 Tab-to-Jump

Fluid cursor navigation suggesting the next logical edit position.

- A "Tab to jump" icon appears when a suggested jump is available
- **Accept:** `Tab` → cursor moves to suggested position

### 4.3 Tab-to-Import

Handles missing dependency imports automatically.

- Detects unimported classes or functions as you type
- Suggests the correct import statement
- **Accept:** `Tab` → completes the word AND adds import at top of file

### 4.4 Tab Settings

| Setting | Options | Description |
|---|---|---|
| Enable/Disable individually | Toggle per feature | Control Autocomplete, Supercomplete, Tab-to-Jump, Tab-to-Import separately |
| Tab Speed | Slow / Default / Fast | Responsiveness of suggestions |
| Highlight Inserted Text | On/Off | Highlights Tab-inserted text |
| Clipboard Context | On/Off | Uses clipboard content to improve suggestions |
| Allow Gitignored Files | On/Off | Enables Tab in `.gitignore`d files (requires git) |

> **Tab completions are unlimited** — they do not consume any quota on any plan.

---

## 5. Review Changes

Accessible via the **"Review Changes"** section in the Agent panel's bottom toolbar (appears once agent starts modifying files).

**Features:**
- Opens a pane showing all file diffs from the current conversation
- Covers changes made by both user and agent
- Each diff is **commentable** (like artifacts) for direct agent feedback

---

## 6. Browser Deep Dive

### 6.1 What the browser agent does

The Browser Subagent operates local Chrome to:
- Test development websites
- Read documentation sources
- Automate browser tasks (dashboard reads, SCM actions, UI testing)
- Capture screenshots and screen recordings as artifacts

**Disable:** Toggle "Browser Tools" in User Settings.

### 6.2 Security Model

Two-layer system controlling URL access:

**Layer 1 — Denylist (server-side):**
- Maintained by Google Superroots BadUrlsChecker service
- Checked via RPC on every navigation attempt
- **Fail-safe:** If service is unavailable → access is DENIED by default
- Cannot be overridden by allowlist

**Layer 2 — Allowlist (local file):**
- Initialized with `localhost` only
- Editable at any time
- "Always Allow" button to add URLs during navigation
- **Denylist always takes precedence** — cannot allowlist a denylisted URL

**Precedence:** Denylist > Allowlist

### 6.3 Chrome Profile Isolation

The browser runs in a **completely separate Chrome profile**:
- No cookies, sessions, or sign-in data shared with your normal Chrome
- Sign-ins within this profile persist across Antigravity sessions
- Appears as a separate dock icon if Chrome was already open
- Profile location configurable in Browser Settings
- To return to normal Chrome: quit the Antigravity browser, relaunch Chrome

### 6.4 Browser Recordings (exclusive to IDE)

- Generated automatically when browser subagent performs actions
- Viewable at the bottom of the Browser step UI
- Saved as a **recording artifact** that loops through agent actions
- Not available in 2.0 (which has screenshots only)

---

## 7. Customizations

IDE Customizations vs 2.0:
- 2.0 has: MCP, Skills, Rules, Workflows, Plugins, Hooks, **Sidecars**
- IDE has: MCP, Skills, Rules, **Workflows (own page)**, Plugins, Hooks (no Sidecars)

### 7.1 MCP

**Canonical path:** `~/.gemini/config/mcp_config.json` (shared with CLI and 2.0)

Access: `...` → Agent panel → "Manage MCP Servers" → "View raw config"

OAuth tokens: `~/.gemini/antigravity/mcp_oauth_tokens.json`

### 7.2 Skills

**Paths:**

| Scope | Path |
|---|---|
| Global | `~/.gemini/antigravity/skills/` (**NOTE: different from 2.0!**) |
| Workspace | `.agents/skills/` |
| Legacy workspace | `.agent/skills/` |

Progressive disclosure: agent reads SKILL.md only when task is relevant.

**SKILL.md frontmatter:**
```yaml
---
name: my-skill
description: What it does and when. Third person, trigger keywords.
---
```

### 7.3 Rules

**File size limit:** 12,000 characters each

| Scope | Path |
|---|---|
| Global | `~/.gemini/GEMINI.md` |
| Workspace | `.agents/rules/` (also supports legacy `.agent/rules/`) |

4 activation modes: Always On, Manual, Model Decision, Glob.

### 7.4 Workflows

**Exclusive feature** of IDE as a first-class customization section (in 2.0 it was part of Rules).

**Invocation:** `/workflow-name` slash command  
**Nesting:** A workflow can call other workflows (`"Call /workflow-2"`)  
**File size limit:** 12,000 characters each  
**Agent-generated:** Agent can create workflows from conversation history

**Running in context:** `@workflows <workflow_name>` in chat

### 7.5 Plugins

**Global path:** `~/.gemini/config/plugins/`  
**Workspace path:** `.agents/plugins/` or `_agents/plugins/`

Structure: `plugin.json` marker + optional skills/rules/mcp_config.json/hooks.json

### 7.6 Hooks

Same 5-event model as CLI and 2.0. Config in `.agents/hooks.json` or `~/.gemini/config/hooks.json`.

---

## 8. Settings

### 8.1 Command Execution & File Access

| Setting | Options |
|---|---|
| Terminal Command Auto Execution | Request Review (default) / Always Proceed |
| Agent Non-Workspace File Access | Enable/disable access outside project folders |

Default allowed paths: project folders + `~/.gemini/antigravity/` (artifacts, knowledge items).

### 8.2 Strict Mode

Enhanced security mode that overrides multiple individual settings simultaneously.

**When Strict Mode is ON, these are enforced:**

| Setting | Forced value |
|---|---|
| Terminal Auto Execution | Always Request Review (allowlist ignored) |
| Browser JS Execution | Always Request Review |
| Artifact Review | Always Request Review |
| Browser URL control | Allowlist/Denylist enforced for images and Read URL |
| File system | Respects `.gitignore` |
| Workspace isolation | Access outside workspace disabled |
| Sandboxing | Automatically activated with network DENIED |

> **Strict Mode + Sandboxing = maximum protection**

### 8.3 Terminal Sandboxing

| Platform | Implementation |
|---|---|
| Linux | nsjail |
| macOS | sandbox-exec (Apple Seatbelt) |

**Default:** Disabled  
**"Sandbox Allow Network" toggle:** Independent network control (separate from file system restrictions)  
**Strict Mode interaction:** Forces sandbox ON + network DENIED

**File system restrictions:** Commands can only write to workspace directory and essential system locations.

---

## 9. Artifacts

| Type | When created | Exclusive |
|---|---|---|
| Implementation Plan | Before changes in Planning mode | — |
| Walkthrough | After task completion | — |
| Screenshots | Browser subagent captures | — |
| **Browser Recordings** | During browser actuation | ✅ **IDE only** |

**Browser Recordings:** Auto-generated, loops through agent's browser actions. Viewable at bottom of Browser step UI, also saved as artifact.

---

## 10. Firebase Studio Migration

### Why migrate
- Local environment control
- True agentic development (beyond code completion)
- Seamless Firebase support (Firebase CLI, App Hosting, Local Emulator Suite)

### Prerequisites
- Antigravity IDE installed
- Node.js v20+
- Firebase CLI v15.10.0+

### Step 1 — Export project

**Option A (Automated):**
1. In Firebase Studio: "Move now" button → Zip & Download
2. Extract locally, open in Antigravity IDE
3. In Agent pane, type:
   ```
   @fbs-to-agy-export
   ```
4. Follow agent guidance

> Recommend Gemini Flash model for token efficiency during migration.

**Option B (Manual):**
```bash
npx firebase-tools@latest studio:export <path>
```
> Optimized for: Next.js, Flutter, Angular. Other workspace types: partial support.

### Step 2 — Preview
1. Run & Debug (left sidebar) → Play button
2. Follow terminal instructions to preview app

### Step 3 — Publish
In Agent chat:
```
Publish my app
```
Agent uses Firebase CLI and App Hosting skills.

### Ongoing work
- Run workflows: `@workflows <workflow_name>`
- Deploy: Agent via skills, or Firebase CLI + GitHub directly

---

## 11. IDE Version History

| Date | Version | Key milestone |
|---|---|---|
| Nov 18, 2025 | 1.11.2 | **Original launch** (as unified Antigravity app) |
| Dec 2025 | 1.12.4 | Gemini 3 Flash + native audio |
| Jan 13, 2026 | 1.14.2 | Agent Skills introduced |
| Jan 23, 2026 | 1.15.6 | Terminal Sandboxing (macOS) |
| Feb 3, 2026 | 1.16.5 | "Secure Mode" → "Strict Mode" rename |
| Feb 19, 2026 | 1.18.3 | Gemini 3.1 Pro, artifact download, ↑↓ input history |
| Mar 9, 2026 | 1.20.5 | AGENTS.md support, auto-continue default |
| Mar 25, 2026 | 1.21.6 | Linux Sandboxing, MCP auth improvements |
| Apr 7, 2026 | 1.22.2 | Unified permissions system |
| May 19, 2026 | 2.0.1 | **First release as "Antigravity IDE"** (split from combined app) |
| May 21, 2026 | 2.0.2 | Installation location fix |
| May 21, 2026 | 2.0.3 | Migration flow from 1.0 (import customizations) |
| Jun 2, 2026 | 2.0.4 | Enterprise auth fix ← **current** |

---

*Last updated: June 2026 — Antigravity IDE v2.0.4*
