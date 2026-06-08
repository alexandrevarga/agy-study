# Antigravity CLI — Quick Reference

> Compact cheatsheet for daily use. All tables are scannable at a glance.

---

## Slash Commands

| Command | Alias | What it does |
|---|---|---|
| `/help` | — | Show help and shortcuts |
| `/settings` | `/config` | Open settings panel |
| `/model` | — | Switch model |
| `/theme` | — | Switch color theme |
| `/quit` | `/exit` | Quit |
| `/new` | — | New conversation |
| `/resume` | — | Resume past conversation |
| `/clear` | — | Clear current conversation |
| `/history` | — | Show conversation history |
| `/add-dir` | — | Add directory to workspace |
| `/open` | — | Open file (shell autocomplete) |
| `/close-dir` | — | Remove directory from workspace |
| `/mcp` | — | Manage MCP servers |
| `/skills` | — | Browse available skills |
| `/permissions` | — | Add/edit/remove permission rules |
| `/diff` | — | Git diff (supports `commit1..commit2`) |
| `/fast` | — | Fast mode (no planning - `/yolo` deprecated) |
| `/planning` | — | Planning mode (review plans) |
| `/usage` | `/quota` | Real-time quota display |
| `/credits` | — | G1 credits panel |
| `/statusline` | — | Manage custom status line |
| `/changelog` | — | Show CLI changelog |
| `/browser` | — | Invoke browser subagent |
| `/teamwork-preview` | — | Multi-agent coordination (Ultra only) |
| `/<skill-name>` | — | Run a skill by name |

---

## Keybindings

| Key | Action |
|---|---|
| `Enter` | Submit message |
| `ctrl+j` | Submit message (alternative) |
| `ctrl+n` | New conversation |
| `ctrl+r` | Toggle Artifact Review panel |
| `ctrl+o` | Expand/collapse tool output |
| `ctrl+k` | Focus input |
| `ctrl+c` | Cancel agent / interrupt stream |
| `ctrl+d` `ctrl+d` | Quit |
| `alt+j` | Teleport to conversation picker |
| `ctrl+f` | Search / find |
| `ctrl+s` | Save |
| `ctrl+z` | Undo |
| `Tab` | Navigate / autocomplete |
| `↑` / `↓` | Navigate input history |
| `PageUp` / `PageDown` | Scroll viewport |
| `ctrl+delete` | Delete session in `/resume` |

---

## settings.json Keys

| Key | Type | Default | Description |
|---|---|---|---|
| `selectedModel` | string | — | Default conversation model |
| `autoAcceptChanges` | boolean | `false` | Auto-accept file edits |
| `toolDiscoveryMode` | `"automatic"\|"manual"` | `"automatic"` | Skills/tool discovery |
| `toolCallMode` | `"auto"\|"manual"` | `"auto"` | Tool execution mode |
| `theme` | string | `"default"` | Color scheme |
| `statusBar` | boolean | `true` | Show status bar |
| `statusLine.command` | string | — | Shell cmd for status line |
| `statusLine.interval_seconds` | number | — | Refresh rate |
| `statusLine.stack_with_default` | boolean | `false` | Stack default + custom (**v1.0.6**) |
| `windowTitle.command` | string | — | Shell cmd for window title |
| `windowTitle.interval_seconds` | number | — | Refresh rate |
| `preferredEditor` | string | `$EDITOR` | Override editor |
| `sandbox.enabled` | boolean | `false` | Enable sandbox |
| `sandbox.allowedNetworkHosts` | string[] | `[]` | Network allowlist in sandbox |
| `UseG1Credits` | boolean | `false` | Auto-use G1 credits (**v1.0.3**) |

---

## Permissions Actions

| Action | Target Format | Match Behavior | Example |
|---|---|---|---|
| `read_file` | Absolute path or dir | Path prefix | `read_file(/home/user/docs)` |
| `write_file` | Absolute path or dir | Path prefix | `write_file(/home/user/project)` |
| `read_url` | Domain or `*` | Domain + subdomains | `read_url(github.com)` |
| `execute_url` | Domain or `*` | Domain + subdomains | `execute_url(api.example.com)` |
| `command` | Command prefix or `*` | Token prefix | `command(git)` |
| `unsandboxed` | Command prefix or `*` | Token prefix | `unsandboxed(npm)` |
| `mcp` | `server/tool`, `server/*`, or `*` | Exact server name | `mcp(github/*)` |

**Precedence:** deny > ask > allow

**Special mode:** `proceed-in-sandbox` — auto-approves sandboxed commands, asks only for bypass attempts.

---

## MCP Config Fields

**Canonical path:** `~/.gemini/config/mcp_config.json`  
**Legacy path:** `~/.gemini/antigravity-cli/mcp_config.json` *(orphan post-v1.0.3)*

| Field | Type | Required | Applies to | Description |
|---|---|---|---|---|
| `command` | string | One of: command/serverUrl/url | stdio | Executable path |
| `serverUrl` | string | One of | HTTP | Remote server URL |
| `url` | string | One of | URL | URL-based server (**v1.0.5**) |
| `args` | string[] | No | stdio | Command arguments |
| `env` | object | No | stdio | Environment variables |
| `cwd` | string | No | stdio | Working directory |
| `headers` | object | No | HTTP | Custom headers |
| `authProviderType` | string | No | HTTP | `"google_credentials"` for ADC |
| `oauth.clientId` | string | No | HTTP | OAuth client ID |
| `oauth.clientSecret` | string | No | HTTP | OAuth client secret |
| `disabled` | boolean | No | Both | Disable without removing |
| `disabledTools` | string[] | No | Both | Tools to suppress |

---

## CLI Flags & Subcommands

| Command | Description |
|---|---|
| `agy` | Launch interactive TUI |
| `agy --model <name>` | Launch with specific model (**v1.0.5**) |
| `agy -p "prompt"` / `agy --print "prompt"` | Headless non-interactive mode (**v1.0.5**) |
| `agy --sandbox` | Force sandbox in headless mode (**v1.0.6**) |
| `agy update` | Update CLI |
| `agy models` | List available models (**v1.0.5**) |
| `agy changelog` | Show changelog (**v1.0.4**) |
| `agy plugin install <name>` | Install plugin |

---

## Environment Variables

| Variable | Effect |
|---|---|
| `AGY_CLI_DISABLE_LATEX` | Disable LaTeX math rendering globally |
| `AGY_CLI_HIDE_ACCOUNT_INFO` | Hide email and plan tier from header |

---

## Key Paths

| What | Path |
|---|---|
| CLI app data | `~/.gemini/antigravity-cli/` |
| **MCP config (current)** | `~/.gemini/config/mcp_config.json` |
| MCP config (legacy, orphan) | `~/.gemini/antigravity-cli/mcp_config.json` |
| settings.json | `~/.gemini/antigravity-cli/settings.json` |
| Conversations (SQLite) | `~/.gemini/antigravity-cli/conversations/` |
| Projects cache | `~/.gemini/antigravity-cli/cache/projects.json` |
| Changelog cache | `~/.gemini/antigravity-cli/cache/CHANGELOG.md` |
| Skills (global) | `~/.gemini/config/skills/` |
| Skills (workspace) | `.agents/skills/` |
| Plugins (global) | `~/.gemini/config/plugins/` |
| Rules (global) | `~/.gemini/GEMINI.md` or `~/.gemini/AGENTS.md` |
| Rules (workspace) | `.agents/rules/` |
| Hooks | `.agents/hooks.json` or `~/.gemini/config/hooks.json` |
| Shared config | `~/.gemini/config/` |

---

## Subagents

| Agent | Type | Capabilities | Invocation |
|---|---|---|---|
| `research` | Built-in | Read-only (files, web, grep) | `invoke_subagent` |
| `self` | Built-in | Full parent capabilities | `invoke_subagent` |
| `browser` | Built-in | Browser interaction | `/browser` only |
| Custom | User-defined | Configurable | `define_subagent` → `invoke_subagent` |

**Workspace modes:** `inherit` (default) · `branch` (isolated) · `share` (shared repo)  
**Max nesting depth:** 10 levels

---

## Hooks Events

| Event | Fires when | Input extras | Output |
|---|---|---|---|
| `PreToolUse` | Before tool call | `toolCall.name`, `toolCall.args`, `stepIdx` | `decision`, `reason`, `permissionOverrides` |
| `PostToolUse` | After tool call | `stepIdx`, `error` | `{}` |
| `PreInvocation` | Before model call | `invocationNum`, `initialNumSteps` | `injectSteps` |
| `PostInvocation` | After tool calls finish | Same as PreInvocation | `injectSteps`, `terminationBehavior` |
| `Stop` | Loop terminates | `executionNum`, `terminationReason`, `fullyIdle` | `decision`, `reason` |

**`decision` values for PreToolUse:** `allow` · `deny` · `ask` · `force_ask`  
**`terminationBehavior` values:** `force_continue` · `terminate` · `""` (default)  
**`decision` value for Stop:** `"continue"` to prevent stop

---

*CLI v1.0.6 — June 2026*
