# Antigravity CLI — Mastering Guide

> **SSOT (Single Source of Truth)** — Compiled from 74 pages of official documentation + full changelog (v1.0.0–v1.0.6). All discrepancies resolved and canonical paths verified against the live filesystem.

---

## 1. What is the CLI

The Antigravity CLI is the **terminal-first surface** of the Antigravity platform. It runs as a full TUI (Terminal User Interface) inside your existing terminal emulator — no browser, no Electron, no GUI required.

**Positioning vs other surfaces:**

| Surface | Nature | Best for |
|---|---|---|
| **CLI** | TUI in terminal | Terminal-first workflows, low overhead, SSH sessions |
| **2.0** | Standalone desktop app | Async tasks, voice, scheduled tasks, Projects model |
| **IDE** | Full AI-powered IDE | Integrated editing with Tab autocomplete + Browser |
| **SDK** | Python framework | Embedding agents in your own applications |

**Key characteristics:**
- Shares the same core agent harness as 2.0 and IDE
- Stateful TUI with persistent conversations
- Full permissions, MCP, skills, hooks, subagents support
- Launched as a completely new product (v1.0.0), separate from the 1.x IDE history

---

## 2. Installation & Authentication

### Install
```bash
# Primary install method (documented)
# Download from antigravity.google/download

# Update
agy update

# Check version
agy --version
```

**Binary location:** `/usr/local/bin/agy` or `~/.local/bin/agy`

### Authentication
- OAuth via Google account (browser-based)
- Supports personal Gmail accounts
- Workspace accounts may have issues → use Gmail if auth fails
- Minimum age requirement enforced
- Geographic availability: Brazil ✅

### Config directory
```
~/.gemini/antigravity-cli/        # CLI private app data
~/.gemini/config/                  # Shared config (CLI + 2.0 + IDE)
```

---

## 3. Interface & Navigation

### TUI Layout
```
┌─────────────────────────────────────────────────────┐
│  Header: account info, plan, model                   │
├─────────────────────────────────────────────────────┤
│                                                       │
│  Conversation viewport (scrollable)                  │
│  - User messages                                      │
│  - Agent responses                                    │
│  - Tool call outputs (collapsible)                   │
│  - Artifacts (with Proceed button when applicable)   │
│                                                       │
├─────────────────────────────────────────────────────┤
│  Status line (customizable)                          │
├─────────────────────────────────────────────────────┤
│  Input box (multi-line, history-aware)               │
└─────────────────────────────────────────────────────┘
```

### Keybindings

| Key | Action |
|---|---|
| `Enter` | Submit message |
| `ctrl+j` | Submit message (alternative) |
| `ctrl+n` | New conversation |
| `ctrl+r` | Toggle Artifact Review panel |
| `ctrl+o` | Expand/collapse tool output in viewport |
| `ctrl+k` | Focus input box |
| `ctrl+c` | Cancel / interrupt agent stream |
| `ctrl+d` `ctrl+d` | Quit |
| `alt+j` | Teleport (jump to conversation picker) |
| `ctrl+f` | Search / find |
| `ctrl+s` | Save |
| `ctrl+z` | Undo |
| `Tab` | Navigate / autocomplete |
| `Up` / `Down` arrows | Navigate input history (added v1.18.3) |
| `PageUp` / `PageDown` | Scroll viewport |
| `ctrl+delete` | Delete session in `/resume` (**NOT** `ctrl+d` — changed v1.0.1) |

> **Resolved discrepancy:** Earlier docs mentioned `ctrl+j` for teleport. Canonical: `alt+j` is teleport, `ctrl+j` is submit.

---

## 4. Slash Commands

Full list (including aliases and commands added post-launch):

| Command | Aliases | Description |
|---|---|---|
| `/help` | — | Show help and shortcuts |
| `/settings` | `/config` | Open settings panel |
| `/model` | — | Switch model |
| `/theme` | — | Switch color theme |
| `/quit` | `/exit` | Quit the CLI |
| `/new` | — | Start new conversation |
| `/resume` | — | Resume a past conversation |
| `/clear` | — | Clear current conversation |
| `/history` | — | Show conversation history |
| `/add-dir` | — | Add directory to workspace (with shell path autocomplete since v1.0.6) |
| `/open` | — | Open file (with shell path autocomplete since v1.0.6) |
| `/close-dir` | — | Remove directory from workspace |
| `/mcp` | — | Manage MCP servers |
| `/skills` | — | Browse available skills |
| `/permissions` | — | Add/edit/remove permission rules (full CRUD since v1.0.5) |
| `/diff` | — | Show git diff (supports `commit1..commit2`, short hashes) |
| `/fast` | — | Switch to Fast mode (no planning, immediate execution - `/yolo` alias deprecated/removed) |
| `/planning` | — | Switch to Planning mode (implementation plans + review) |
| `/usage` | — | Show quota usage (forces real-time reload) |
| `/quota` | — | Alias for `/usage` |
| `/credits` | — | Open credits panel (G1 credits, added v1.0.3) |
| `/statusline` | — | Manage custom status line (enable/disable/reset/delete) |
| `/changelog` | — | Show CLI changelog (cached at `~/.gemini/antigravity-cli/cache/CHANGELOG.md`) |
| `/browser` | — | Invoke browser subagent |
| `/teamwork-preview` | — | Multi-agent coordination (Ultra plan only) |
| `/<skill-name>` | — | Skills generate their own slash commands automatically |

> **Tab/autocomplete:** Fuzzy and partial matching as of v1.0.6 — e.g., `/el` shows `/help` and `/model`.

---

## 5. Configuration — settings.json

**Canonical path (current):** `~/.gemini/antigravity-cli/settings.json`

### All settings keys

| Key | Type | Description |
|---|---|---|
| `selectedModel` | string | Default model for conversations |
| `autoAcceptChanges` | boolean | Auto-accept agent file edits without prompting |
| `toolDiscoveryMode` | `"automatic"` \| `"manual"` | How tools/skills are discovered |
| `toolCallMode` | `"auto"` \| `"manual"` | Execution mode: auto runs tools silently, manual prompts |
| `theme` | string | Color scheme name |
| `statusBar` | boolean | Show/hide the status bar |
| `statusLine` | object | Custom status line configuration (see §11) |
| `windowTitle` | object | Custom window title configuration (see §11) |
| `preferredEditor` | string | Override `$EDITOR` for file editing |
| `sandbox` | object | Sandboxing configuration (see §13) |
| `UseG1Credits` | boolean | Auto-use G1 credits when standard quota exhausted (added v1.0.3) |

### Example settings.json
```json
{
  "selectedModel": "gemini-3.1-pro",
  "autoAcceptChanges": false,
  "toolDiscoveryMode": "automatic",
  "toolCallMode": "auto",
  "theme": "default",
  "statusBar": true,
  "statusLine": {
    "command": "echo 'branch: $(git branch --show-current 2>/dev/null)'",
    "interval_seconds": 10,
    "stack_with_default": false
  },
  "windowTitle": {
    "command": "echo 'AGY | $(basename $PWD)'",
    "interval_seconds": 30
  },
  "preferredEditor": "nvim",
  "sandbox": {
    "enabled": false,
    "allowedNetworkHosts": []
  },
  "UseG1Credits": false
}
```

---

## 6. Permissions System

### Architecture
Three ordered lists evaluated on every sensitive action:

```
DENY  →  if matched, block immediately (no override)
  ↓
ASK   →  if matched, prompt the user
  ↓
ALLOW →  if matched, execute silently
```

**Precedence:** deny > ask > allow (deny always wins)

### Permission Actions

| Action | Target Format | Example |
|---|---|---|
| `read_file` | Absolute path or directory | `read_file(/home/user/docs)` |
| `write_file` | Absolute path or directory | `write_file(/home/user/project)` |
| `read_url` | Domain name or `*` | `read_url(github.com)` |
| `execute_url` | Domain name or `*` | `execute_url(api.example.com)` |
| `command` | Command prefix or `*` | `command(git)` |
| `unsandboxed` | Command prefix or `*` | `unsandboxed(npm)` |
| `mcp` | `serverName/toolName`, `serverName/*`, or `*` | `mcp(github/*)` |

### Special permission modes
- **`proceed-in-sandbox`**: Auto-approves commands running inside the sandbox; only asks when the command tries to bypass it. (added v1.0.1)

### Managing permissions
- Via `/permissions` command: full add/edit/remove CRUD UI inside the TUI (v1.0.5)
- Via `settings.json` → `permissions` key
- Via project-level permissions (merged with user and CLI settings as of v1.0.5)
- Permissions bubble up from subagents to parent agent's UI

### Permission scopes (merged in v1.0.5)
1. Project-level permissions
2. User settings (shared with 2.0/IDE)
3. CLI `settings.json` permissions

---

## 7. MCP Configuration

### ⚠️ Canonical path (post-migration)

```
CURRENT  (v1.0.3+): ~/.gemini/config/mcp_config.json     ← USE THIS
LEGACY   (pre-v1.0.3): ~/.gemini/antigravity-cli/mcp_config.json  ← may still exist, ignored
```

The CLI migrated to the shared config path in v1.0.3 to align with 2.0 and IDE. If you have both files, only `~/.gemini/config/mcp_config.json` is authoritative for the TUI disable/enable actions.

### Config structure

```json
{
  "mcpServers": {
    "serverName": {
      "command": "/path/to/executable",
      "args": ["--transport", "stdio"],
      "env": {
        "API_KEY": "your-api-key"
      },
      "url": "https://...",
      "serverUrl": "https://...",
      "cwd": "/working/directory",
      "headers": {
        "Authorization": "Bearer TOKEN"
      },
      "authProviderType": "google_credentials",
      "oauth": {
        "clientId": "your-client-id",
        "clientSecret": "your-client-secret"
      },
      "disabled": false,
      "disabledTools": ["toolName1", "toolName2"]
    }
  }
}
```

### Transport options

| Field | Transport | Notes |
|---|---|---|
| `command` | stdio | Path to executable |
| `serverUrl` | Streamable HTTP | Remote server URL |
| `url` | URL-based | Added v1.0.5 |
| `args` | stdio only | Command-line arguments |
| `env` | stdio only | Environment variables for process |
| `headers` | HTTP only | Custom headers (e.g., API keys) |

### Authentication options

**Google Application Default Credentials (ADC):**
```json
{
  "mcpServers": {
    "my-gcp-service": {
      "serverUrl": "https://example.googleapis.com/mcp/",
      "authProviderType": "google_credentials"
    }
  }
}
```
Requires: `gcloud auth application-default login`

**OAuth (auto-DCR):**
```json
{ "mcpServers": { "server": { "serverUrl": "https://api.example.com/mcp/" } } }
```

**OAuth (manual credentials):**
```json
{
  "mcpServers": {
    "server": {
      "serverUrl": "https://api.example.com/mcp/",
      "oauth": { "clientId": "...", "clientSecret": "..." }
    }
  }
}
```
OAuth callback: `https://antigravity.google/oauth-callback`

### MCP startup behavior (v1.0.4+)
MCP servers initialize in **parallel** — slow/hanging custom servers no longer block fast-starting servers (like local plugins).

### Managing via TUI
- `/mcp` command: browse, enable/disable, configure servers
- After v1.0.3: TUI disable correctly writes to `~/.gemini/config/mcp_config.json`

---

## 8. Skills & Plugins

### Skills

**What:** Reusable packages of instructions. Each skill generates a `/skill-name` slash command.

**Discovery:** Progressive disclosure — agent sees skill name+description at conversation start, reads full SKILL.md only when relevant task detected.

**Paths:**

| Scope | Path |
|---|---|
| Global | `~/.gemini/config/skills/<skill-folder>/` |
| Workspace | `.agents/skills/<skill-folder>/` |
| Legacy workspace | `.agent/skills/<skill-folder>/` (backward compat) |

**Structure:**
```
.agents/skills/my-skill/
├── SKILL.md          # Required
├── scripts/          # Optional helper scripts
├── examples/         # Optional reference implementations
└── resources/        # Optional templates and assets
```

**SKILL.md frontmatter:**
```yaml
---
name: my-skill          # Optional, defaults to folder name
description: What it does and when to use it. Use third person.
---

# Skill Title

## When to use this skill
## How to use it
```

> **Tip for descriptions:** Include trigger keywords that help the agent recognize relevance. E.g., `"Generates unit tests for Python code using pytest conventions."`

**Best practices:**
- One skill = one focused task
- Use scripts as black boxes (agent runs `--help` first, not reading source)
- Include decision trees for complex skills

### Plugins

**What:** Namespaced bundles grouping skills + rules + MCP servers + hooks.

**Install path (v1.0.2+):** `~/.gemini/config/plugins/` (shared with 2.0/IDE)  
**Legacy path (pre-v1.0.2):** `~/.gemini/antigravity-cli/plugins/`

**Structure:**
```
plugins/<plugin-name>/
├── plugin.json           # Required marker
├── mcp_config.json       # Optional MCP definitions
├── hooks.json            # Optional hooks
├── skills/<skill-name>/SKILL.md
└── rules/<rule-name>.md
```

**plugin.json:**
```json
{ "name": "my-plugin" }
```
`name` is optional — defaults to directory name.

**Install via CLI:**
```bash
agy plugin install <name>
```
As of v1.0.2, installs to `~/.gemini/config/` (shared), not the private CLI data dir.

**Build with Google plugins:** Available at Customizations > Plugins in the UI.

---

## 9. Rules & Workflows

### Rules

**Global rule file:** `~/.gemini/GEMINI.md`  
**Also accepted:** `~/.gemini/AGENTS.md` (added in v1.20.5 of the codebase, both are valid)  
**Workspace rules:** `.agents/rules/<rule-name>.md`  
**Legacy workspace:** `.agent/rules/` (backward compat)  
**File size limit:** 12,000 characters each

**4 activation modes:**

| Mode | Behavior |
|---|---|
| **Always On** | Applied to every conversation automatically |
| **Manual** | Activated via `@mention` in the input box |
| **Model Decision** | Agent decides based on relevance to current task |
| **Glob** | Applied when files matching the pattern are in context (e.g., `*.ts`, `src/**/*.py`) |

**`@` mentions in rules files:**
- `@filename` — relative to the rules file location
- `@/path/to/file.md` — absolute, then falls back to `workspace/path/to/file.md`

### Workflows

**What:** Sequences of steps guiding the agent through repetitive multi-step tasks.

**Invocation:** `/workflow-name` slash command  
**File location:** `.agents/workflows/` (workspace) or `~/.gemini/config/workflows/` (global)  
**File format:** Markdown with title, description, and steps  
**File size limit:** 12,000 characters each  
**Nesting:** Workflows can call other workflows: `"Call /workflow-2"`

**Agent-generated workflows:** Ask the agent to generate a workflow after working through a process manually — it uses conversation history as source.

---

## 10. Hooks

**Config file:** `hooks.json` in `.agents/` or `~/.gemini/config/`

### 5 Events

| Event | When | Matcher |
|---|---|---|
| `PreToolUse` | Before tool execution | Tool name (regex) |
| `PostToolUse` | After tool completes | Tool name (regex) |
| `PreInvocation` | Before model is called | N/A (ignored) |
| `PostInvocation` | After tool calls finish | N/A (ignored) |
| `Stop` | When execution loop terminates | N/A (ignored) |

### Matcher patterns
```
""  or  "*"          → Match all tools
"run_command"        → Exact match
"run_command|view_file" → Either tool
"browser_.*"         → Regex prefix match
```

### hooks.json example
```json
{
  "my-linter": {
    "PostToolUse": [{
      "matcher": "write_to_file|replace_file_content",
      "hooks": [{
        "type": "command",
        "command": "./scripts/lint.sh",
        "timeout": 10
      }]
    }]
  }
}
```

### I/O Contract

**Common input fields (all hooks):**
```json
{
  "conversationId": "uuid",
  "workspacePaths": ["/path/to/workspace"],
  "transcriptPath": "/path/to/transcript.jsonl",
  "artifactDirectoryPath": "/path/to/artifacts"
}
```

**PreToolUse output:**
```json
{
  "decision": "allow|deny|ask|force_ask",
  "reason": "Optional explanation",
  "permissionOverrides": ["command(npm test)"]
}
```

**PreInvocation output (inject steps):**
```json
{
  "injectSteps": [
    { "ephemeralMessage": "Remember to lint before committing" },
    { "userMessage": "Also check for TODO comments" }
  ]
}
```

**PostInvocation output:**
```json
{
  "injectSteps": [],
  "terminationBehavior": "force_continue|terminate|"
}
```

**Stop output:**
```json
{
  "decision": "continue",
  "reason": "Agent must finish the failing test before stopping"
}
```

---

## 11. Status Line & Window Title

### Status Line

Displayed below the conversation viewport. Runs a shell command at intervals and renders the output.

```json
"statusLine": {
  "command": "git branch --show-current 2>/dev/null | xargs -I{} echo 'branch: {}'",
  "interval_seconds": 10,
  "stack_with_default": false
}
```

| Field | Type | Description |
|---|---|---|
| `command` | string | Shell command, output rendered as status line |
| `interval_seconds` | number | Refresh interval |
| `stack_with_default` | boolean | **NEW v1.0.6** — if true, renders both default Antigravity status line AND custom output stacked vertically |

**Manage via `/statusline` command:**
- `/statusline enable` / `/statusline on`
- `/statusline disable` / `/statusline off`
- `/statusline reset`
- `/statusline delete`
- `/statusline help`

### Window Title

Sets the terminal window/tab title.

```json
"windowTitle": {
  "command": "echo \"AGY | $(basename $PWD) | $(git branch --show-current 2>/dev/null)\"",
  "interval_seconds": 30
}
```

---

## 12. Subagents

### Built-in subagents

| Name | Capabilities | Notes |
|---|---|---|
| `research` | Read-only (files, web, grep) | No write, no commands |
| `self` | Full parent capabilities | Same tools + model |
| `browser` | Browser interaction | Invoked via `/browser` only |

### Spawning subagents

**Tool:** `invoke_subagent`
```json
{
  "Subagents": [{
    "TypeName": "research",
    "Role": "Documentation Researcher",
    "Prompt": "Find all references to MCP configuration in the codebase",
    "Workspace": "inherit"
  }]
}
```

**Workspace modes:**

| Mode | Behavior |
|---|---|
| `inherit` | Shares parent workspace (default) |
| `branch` | New isolated workspace, no storage sharing |
| `share` | Shares underlying repo (like git worktree) |

### Custom subagents

**Tool:** `define_subagent`
```python
define_subagent(
  name="custom-agent",
  description="What it does and when to use it",
  system_prompt="Detailed instructions...",
  enable_mcp_tools=False,
  enable_write_tools=False,
  enable_subagent_tools=False
)
```

### Subagent lifecycle
- States: **Running → Idle → Killed**
- Idle agents can be re-awakened by any agent (not just parent) via `send_message`
- Inter-agent communication: `send_message` tool with recipient conversation ID
- Max nesting depth: **10 levels** (hard limit)
- Permissions: subagents inherit parent's safety configuration
- Approval bubbling: subagent permission requests bubble up to parent's UI

### /teamwork-preview
Multi-agent coordination workflow. **Ultra plan only ($200/mo)**. Manages error recovery, auto-retries, and team coordination automatically.

---

## 13. Sandbox

### What it does
Provides kernel-level isolation for terminal commands executed by the agent. Prevents writes outside the workspace and controls network access.

### Platform support

| Platform | Implementation | Status |
|---|---|---|
| **Linux** | nsjail (process isolation) | Preview (added v1.21.6 / v1.0.x CLI) |
| **macOS** | sandbox-exec (Apple Seatbelt) | Preview (added v1.15.6) |
| **Windows** | Coming soon | Not available |

**Default:** Disabled. May change in future releases.

### Configuration

```json
"sandbox": {
  "enabled": true,
  "allowedNetworkHosts": ["api.github.com", "registry.npmjs.org"]
}
```

### Permission mode integration
- `proceed-in-sandbox`: Auto-approves commands inside sandbox; asks only for bypass attempts
- When sandbox + deny-all = maximum isolation

### Interaction with Strict Mode
When Strict Mode is enabled (IDE), sandbox is automatically activated with network access denied.

---

## 14. AI Credits & Quota

### Quota tiers

| Plan | Refresh | Weekly limit | Third-party models |
|---|---|---|---|
| Basic (free) | Weekly | Yes | ❌ |
| Google AI Pro | Every 5h | Higher | ❌ |
| Google AI Ultra | Every 5h | Highest | ✅ |

**Tab completions are unlimited** (don't consume quota on any plan).

### Commands

- `/usage` / `/quota` — real-time quota display (forces fresh load)
- `/credits` — G1 credits panel, direct link to purchase

### G1 Credits (overage)

Available for Pro and Ultra users. Consumed at standard Gemini Enterprise Agent Platform pricing.

**`UseG1Credits` setting** (added v1.0.3):
```json
{ "UseG1Credits": true }
```

When enabled, the CLI automatically uses G1 credits when standard quota is exhausted, then switches back to baseline quota after the next refresh.

**Status bar display:** Remaining G1 credits shown in real-time in the status bar (v1.0.3+).

### Not supported
- Bring-your-own-key (BYOK)
- Bring-your-own-endpoint (BYOE)
- Organizational tiers via contract

---

## 15. CLI Flags & Environment Variables

### CLI flags

| Flag | Description | Added |
|---|---|---|
| `--model <name>` | Set model at launch | v1.0.5 |
| `--sandbox` | Force sandbox in headless/print mode | v1.0.6 |
| `-p` / `--print` | Non-interactive headless output mode | v1.0.5 |

### CLI subcommands

| Subcommand | Description |
|---|---|
| `agy update` | Update to latest version |
| `agy models` | List available models | 
| `agy changelog` | Show changelog (uses same cache as `/changelog`) |
| `agy plugin install <name>` | Install a plugin |

### Environment variables

| Variable | Effect |
|---|---|
| `AGY_CLI_DISABLE_LATEX` | Disables LaTeX math rendering globally |
| `AGY_CLI_HIDE_ACCOUNT_INFO` | Hides email and plan tier from the header |

---

## 16. Best Practices

### From official docs
- Keep skills focused — one skill, one task
- Write skill descriptions in third person with trigger keywords
- Use scripts as black boxes (let agent discover via `--help`)
- Use hooks for enforcing linters and safety gates automatically
- Use `proceed-in-sandbox` for high-trust sandboxed workflows
- Use workspace-level configs (`.agents/`) for team conventions, global for personal utilities

### From changelog insights
- **AGENTS.md is valid** as an alternative to GEMINI.md — use whichever fits your convention
- **MCP servers initialize in parallel** (v1.0.4) — a slow custom server won't block others at startup
- **Rules exclusion patterns** must be in `rules.json` — they were silently ignored before v1.0.4 fix
- **Skill-derived slash commands must be selected via autocomplete** — don't type and Enter without selecting (bug fixed in v1.0.4)
- **Use `/resume` with `ctrl+delete`** to delete sessions — `ctrl+d` is the quit shortcut
- **`stack_with_default: true`** to see both the default AGY status and your custom output

---

## 17. Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| MCP servers not loading | Using legacy path for TUI disable | Ensure `~/.gemini/config/mcp_config.json` is up to date |
| Skill slash command doesn't execute | Bug pre-v1.0.4 | Ensure CLI is updated to v1.0.4+ |
| `/resume` `ctrl+d` quits app | `ctrl+d` is quit shortcut | Use `ctrl+delete` for session deletion |
| `wordLeft` hangs at beginning of input | Bug pre-v1.0.3 | Ensure CLI is updated |
| Rules not being applied | `rules.json` exclusion bug pre-v1.0.4 | Update CLI; check rules.json patterns |
| Slow startup | VCS detection in startup path pre-v1.0.4 | Resolved in v1.0.4+ |
| Authentication hangs | OAuth token persistence issue | Resolved in v1.0.1+; try re-auth |
| Long conversation slow | Performance regression | Resolved in v1.15.8+; update CLI |
| `$EDITOR` with `=` in args fails | Parsing bug pre-v1.0.3 | Resolved in v1.0.3+ |
| Token limit hit early | Token accounting bug | Resolved in v1.20.5+ |
| `alt+j` (teleport) does not work | Intercepted by GNOME tiling or missing Ghostty Option-as-Meta | Use `/resume` instead |
| `ctrl+n` does not start new conversation | Captured by TUI input box readline history navigation | Use `/new` or `/clear` instead |
| `ctrl+o` blinks screen / does nothing | Input box has focus or no tool output is selected | Select/focus a tool output in the viewport first |
| Stuck in Tmux copy mode | `Esc` cancels active stream/input | Press `q` to exit Tmux copy/edit mode cleanly |

---

## 18. Changelog & Evolution

### CLI version history (v1.0.0 → v1.0.6)

| Version | Key additions |
|---|---|
| **1.0.0** | Initial CLI release |
| **1.0.1** | `proceed-in-sandbox`, plugin discovery, `/quota` alias, consumer onboarding |
| **1.0.2** | `AGY_CLI_HIDE_ACCOUNT_INFO`, plugin path → `~/.gemini/config/`, `/statusline` fix |
| **1.0.3** | G1 credits full support, `/credits` panel, **MCP path migration** to `~/.gemini/config/mcp_config.json` |
| **1.0.4** | SQLite conversations, LaTeX rendering, `projects.json` centralized, rules.json fix, MCP parallel init |
| **1.0.5** | `--model` flag, `/permissions` CRUD, `url` in mcp_config, shared permissions integration |
| **1.0.6** | Fuzzy slash commands, `stack_with_default`, `--sandbox` flag, shell path autocomplete in `/open`/`/add-dir` |

### Broader platform history (IDE/2.0 codebase, relevant to CLI)

| Date | Milestone |
|---|---|
| Nov 18, 2025 | Original Antigravity launch (v1.11.2) |
| Jan 13, 2026 | Agent Skills introduced (v1.14.2) |
| Jan 23, 2026 | Terminal Sandboxing — macOS (v1.15.6) |
| Feb 3, 2026 | "Secure Mode" renamed to "Strict Mode" (v1.16.5) |
| Mar 9, 2026 | AGENTS.md support added (v1.20.5) |
| Mar 25, 2026 | Linux Sandboxing (v1.21.6) |
| Apr 7, 2026 | Unified permissions system (v1.22.2) |
| May 19, 2026 | Antigravity 2.0 + IDE split into separate products |
| Jun 3, 2026 | Latest: 2.0 v2.0.11, IDE v2.0.4 |

---

*Last updated: June 2026 — based on CLI v1.0.6 documentation and changelog*
