# 2.0 Study Plan

> Progresso: 0/10 módulos concluídos

Invoke `/study-2.0` to start or resume your session. The agent reads from `2.0/mastering-guide.md`.

---

## Módulo 1 — O que é o Antigravity 2.0
- [ ] Posicionamento: standalone app (não é IDE, não é CLI)
- [ ] Diferenciadores vs CLI e IDE
- [ ] Plataformas suportadas (Linux Fedora 36+)
- [ ] Histórico: launch Nov 2025 → split Mai 2026

## Módulo 2 — Projects Model & Worktrees
- [ ] O que é um Project e como mapeia para diretórios locais
- [ ] Worktrees: múltiplas branches simultâneas
- [ ] Por que o 2.0 é o único surface com suporte nativo a worktrees

## Módulo 3 — Settings Hierarchy
- [ ] Global → Project → Conversation (cada nível sobrescreve o pai)
- [ ] Como acessar settings em cada escopo
- [ ] Agent Settings: Terminal Auto Execution, Non-Workspace File Access

## Módulo 4 — Customizations: MCP, Skills, Rules
- [ ] MCP: mesmo schema que CLI, path compartilhado ~/.gemini/config/
- [ ] Skills: paths global e workspace, progressive disclosure
- [ ] Rules: 4 modos de ativação, 12.000 char limit

## Módulo 5 — Customizations: Workflows, Plugins, Hooks
- [ ] Workflows: /workflow-name, nesting, agent-generated
- [ ] Plugins: plugin.json bundle, paths
- [ ] Hooks: 5 eventos, I/O contract

## Módulo 6 — Sidecars (exclusivo 2.0)
- [ ] O que são sidecars e por que existem
- [ ] sidecar.json: command, args, env, restart
- [ ] agentapi CLI: sidecar se comunica com o agent
- [ ] Auto-restart em crash

## Módulo 7 — Agent Capabilities
- [ ] Permissions: mesmo engine deny/ask/allow
- [ ] Subagents: built-ins, define_subagent, 10 níveis
- [ ] /teamwork-preview: Ultra only, multi-agent orchestration

## Módulo 8 — Artifacts & Artifact Review
- [ ] 3 tipos: Implementation Plan, Walkthrough, Screenshots
- [ ] Planning Mode vs Fast Mode
- [ ] Como comentar em artefatos para redirecionar o agent
- [ ] Diferença de acesso por surface (sidebar vs ctrl+r)

## Módulo 9 — Features Exclusivas
- [ ] Voice transcription (nativa)
- [ ] Scheduled Tasks (cron-like)
- [ ] /browser subagent
- [ ] Worktrees (revisão prática)

## Módulo 10 — Build with Google & Models
- [ ] MCP Store: 35+ integrações disponíveis
- [ ] Modelos disponíveis por plano
- [ ] Terceiros (Claude, GPT): Ultra only
- [ ] Tab completions: ilimitadas
