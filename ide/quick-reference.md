# Antigravity IDE — Quick Reference

---

## Key Paths

| What | Path |
|---|---|
| App data | `~/.gemini/antigravity/` |
| MCP config | `~/.gemini/config/mcp_config.json` |
| OAuth tokens | `~/.gemini/antigravity/mcp_oauth_tokens.json` |
| **Skills (global)** | `~/.gemini/antigravity/skills/` (**≠ 2.0**) |
| Skills (workspace) | `.agents/skills/` |
| Plugins (global) | `~/.gemini/config/plugins/` |
| Rules (global) | `~/.gemini/GEMINI.md` |
| Rules (workspace) | `.agents/rules/` |
| Hooks | `.agents/hooks.json` |

---

## Tab Features

| Feature | What it does | Accept key |
|---|---|---|
| **Supercomplete** | File-wide suggestions (multi-location edits) | `Tab` |
| **Tab-to-Jump** | Move cursor to next logical edit position | `Tab` |
| **Tab-to-Import** | Detect + add missing imports automatically | `Tab` |

**Tab settings:** Speed (Slow/Default/Fast), Clipboard Context, Allow Gitignored Files, Highlight Inserted Text  
**Quota:** Unlimited on ALL plans

---

## Browser Security Model

```
Navigation request
       │
       ▼
  Denylist check (Google Superroots RPC)
  ├── Matched → BLOCKED (cannot override)
  ├── Service down → BLOCKED (fail-safe)
  └── Not matched → ▼
  
  Allowlist check (local file, starts with localhost)
  ├── Matched → ALLOWED
  └── Not matched → Prompt "Always Allow?" button
```

**Chrome profile:** Completely isolated. No shared cookies/sessions. Persists sign-ins between sessions.

---

## Strict Mode Enforcement Table

| Setting | Normal | Strict Mode |
|---|---|---|
| Terminal Auto Execution | Configurable | Always Request Review |
| Browser JS Execution | Configurable | Always Request Review |
| Artifact Review | Configurable | Always Request Review |
| Terminal allowlist | Respected | Ignored |
| .gitignore | Optional | Enforced |
| Workspace isolation | Optional | Enforced |
| Sandboxing | Optional | Auto-ON + network DENIED |

---

## Customizations: IDE vs 2.0

| Type | IDE | 2.0 |
|---|---|---|
| MCP | ✅ `~/.gemini/config/` | ✅ Same |
| Skills | ✅ `~/.gemini/antigravity/skills/` | ✅ `~/.gemini/config/skills/` |
| Rules | ✅ Same paths | ✅ Same |
| Workflows | ✅ First-class page | ✅ (part of Rules section) |
| Plugins | ✅ Same | ✅ Same |
| Hooks | ✅ Same | ✅ Same |
| Sidecars | ❌ Not available | ✅ Exclusive |

---

## Artifacts

| Type | When | IDE-exclusive |
|---|---|---|
| Implementation Plan | Before changes (Planning mode) | — |
| Walkthrough | After task completion | — |
| Screenshots | Browser captures | — |
| **Browser Recordings** | During browser actuation | ✅ YES |

---

## Firebase Studio Migration

| Step | Action |
|---|---|
| **1A Auto** | Firebase Studio → Zip & Download → Open in IDE → `@fbs-to-agy-export` |
| **1B Manual** | `npx firebase-tools@latest studio:export <path>` |
| **2 Preview** | Run & Debug → Play → follow terminal |
| **3 Publish** | Agent chat: `"Publish my app"` |

**Prerequisites:** Node.js v20+, Firebase CLI v15.10.0+  
**Optimized for:** Next.js, Flutter, Angular

---

## Settings Toggles

| Setting | Options |
|---|---|
| Terminal Command Auto Execution | Request Review / Always Proceed |
| Agent Non-Workspace File Access | Enable / Disable |
| Strict Mode | Enable / Disable |
| Enable Terminal Sandboxing | Enable / Disable |
| Sandbox Allow Network | Enable / Disable (independent toggle) |
| Browser Tools | Enable / Disable all browser tools |

---

## IDE Version Timeline

| Date | Version | Milestone |
|---|---|---|
| Nov 2025 | 1.11.2 | Original launch |
| Jan 2026 | 1.14.2 | Skills introduced |
| Jan 2026 | 1.15.6 | macOS Sandboxing |
| Feb 2026 | 1.16.5 | Strict Mode rename |
| Mar 2026 | 1.21.6 | Linux Sandboxing |
| May 19, 2026 | 2.0.1 | IDE product split |
| Jun 2, 2026 | 2.0.4 | Current |

---

*IDE v2.0.4 — June 2026*
