# CLI Study Plan

> Progresso: 3/14 módulos concluídos

Invoke `/study-cli` to start or resume your session. The agent reads from `cli/mastering-guide.md`.

---

## Módulo 1 — Interface & Navegação
- [x] Layout do TUI (header, viewport, statusbar, input)
- [x] Keybindings canônicos (tabela completa com correções do changelog)
- [x] Navegação entre conversas (alt+j teleport, ctrl+n, /resume)

## Módulo 2 — Slash Commands
- [x] Mapa completo de comandos e aliases
- [x] Fuzzy matching (v1.0.6) e autocomplete de slash commands
- [x] Skill-derived slash commands (skills geram /skill-name automaticamente)

## Módulo 3 — Configuração (settings.json)
- [x] Todos os 13 keys canônicos + UseG1Credits
- [x] autoAcceptChanges, toolCallMode, toolDiscoveryMode
- [x] Localização: ~/.gemini/antigravity-cli/settings.json

## Módulo 4 — Sistema de Permissões
- [ ] Engine deny/ask/allow — precedência e funcionamento
- [ ] Tabela de actions (read_file, write_file, command, mcp, etc.)
- [ ] /permissions CRUD via TUI
- [ ] proceed-in-sandbox mode

## Módulo 5 — MCP Configuration
- [ ] Path canônico migrado: ~/.gemini/config/mcp_config.json
- [ ] Estrutura completa do config (todos os campos)
- [ ] Transportes: stdio, serverUrl, url
- [ ] Autenticação: ADC, OAuth automático, OAuth manual
- [ ] Inicialização paralela (v1.0.4)

## Módulo 6 — Skills
- [ ] Progressive disclosure e como o agent lê SKILL.md
- [ ] Paths: global (~/.gemini/config/skills/) e workspace (.agents/skills/)
- [ ] SKILL.md frontmatter: name, description
- [ ] Boas práticas de escrita de skills

## Módulo 7 — Plugins
- [ ] O que é um plugin vs skill
- [ ] Path canônico: ~/.gemini/config/plugins/ (migrado em v1.0.2)
- [ ] Estrutura: plugin.json + mcp_config.json + hooks.json + skills + rules
- [ ] agy plugin install

## Módulo 8 — Rules & Workflows
- [ ] GEMINI.md vs AGENTS.md (ambos válidos desde v1.20.5)
- [ ] 4 modos de ativação: Always On, Manual, Model Decision, Glob
- [ ] Workflows: /workflow-name, nesting, agent-generated
- [ ] @mentions dentro de rules files

## Módulo 9 — Hooks
- [ ] 5 eventos: PreToolUse, PostToolUse, PreInvocation, PostInvocation, Stop
- [ ] I/O contract de cada evento
- [ ] Matchers: exato, wildcard, regex
- [ ] Exemplo prático: hook de linter pós-escrita

## Módulo 10 — Status Line & Window Title
- [ ] statusLine.command e interval_seconds
- [ ] stack_with_default (v1.0.6) — stacking default + custom
- [ ] /statusline subcommands: enable/disable/reset/delete
- [ ] windowTitle config

## Módulo 11 — Subagents
- [ ] 3 built-ins: research, self, browser
- [ ] invoke_subagent: workspace modes (inherit/branch/share)
- [ ] define_subagent: toolsets configuráveis
- [ ] Ciclo de vida: Running → Idle → Killed
- [ ] Max nesting: 10 níveis, permission bubbling

## Módulo 12 — Sandbox
- [ ] Linux (nsjail) vs macOS (sandbox-exec)
- [ ] sandbox.enabled + allowedNetworkHosts
- [ ] Interação com o sistema de permissões
- [ ] proceed-in-sandbox vs full isolation

## Módulo 13 — AI Credits & Quota
- [ ] Tiers: Basic/Pro/Ultra — refresh rates
- [ ] UseG1Credits setting + display na status bar
- [ ] /usage vs /quota vs /credits
- [ ] Tab completions são ilimitadas em todos os planos

## Módulo 14 — Flags, Env Vars & Best Practices
- [ ] CLI flags: --model, --sandbox, -p/--print
- [ ] Subcommands: agy update, agy models, agy changelog
- [ ] Env vars: AGY_CLI_DISABLE_LATEX, AGY_CLI_HIDE_ACCOUNT_INFO
- [ ] Best practices do changelog + troubleshooting
