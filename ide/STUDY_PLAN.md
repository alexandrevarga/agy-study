# IDE Study Plan

> Progresso: 0/8 módulos concluídos

Invoke `/study-ide` to start or resume your session. The agent reads from `ide/mastering-guide.md`.

---

## Módulo 1 — O que é o Antigravity IDE
- [ ] Dois modos de AI: Agent vs Tab (fundamentalmente diferentes)
- [ ] Surfaces: Editor (VS Code base) + Browser integrado
- [ ] Diferenciadores exclusivos: Tab, Browser Recordings, Strict Mode
- [ ] Histórico: v1.11.2 (Nov 2025) → split em IDE próprio (Mai 2026)

## Módulo 2 — Tab Features
- [ ] Supercomplete: sugestões file-wide, edita múltiplos pontos simultaneamente
- [ ] Tab-to-Jump: cursor navega para próxima posição lógica de edição
- [ ] Tab-to-Import: detecta imports faltando e adiciona automaticamente
- [ ] Settings: Tab Speed, Clipboard Context, Allow Gitignored Files
- [ ] Tab completions são ilimitadas (não consomem quota)

## Módulo 3 — Agent Side Panel & Review Changes
- [ ] Localização: lado direito do editor
- [ ] Bottom toolbar: file changes, terminal processes, artifacts
- [ ] Review Changes: pane com todos os diffs da conversa, comentável

## Módulo 4 — Browser: Modelo de Segurança
- [ ] Denylist (Google Superroots RPC) vs Allowlist (local file)
- [ ] Fail-safe: se serviço indisponível → acesso NEGADO
- [ ] Precedência: Denylist sempre vence Allowlist
- [ ] Chrome Profile isolado: sem cookies compartilhados
- [ ] "Always Allow" button para adicionar URLs

## Módulo 5 — Browser Recordings (exclusivo IDE)
- [ ] Como são geradas automaticamente durante atuação do browser agent
- [ ] Visualização: bottom do Browser step UI
- [ ] Salvas como artefatos que loopeiam as ações do agent
- [ ] Por que o 2.0 não tem (tem só screenshots)

## Módulo 6 — Strict Mode & Sandboxing
- [ ] O que o Strict Mode força (tabela completa de enforcement)
- [ ] Strict Mode vs configurações individuais
- [ ] Terminal Sandboxing: nsjail (Linux) / sandbox-exec (macOS)
- [ ] "Sandbox Allow Network" toggle independente
- [ ] Strict Mode = sandbox ON + network DENIED automaticamente

## Módulo 7 — Customizations (diferenças vs 2.0)
- [ ] Workflows: first-class no IDE (própria página), não sub-item de Rules
- [ ] Skills global path diferente: ~/.gemini/antigravity/skills/ (≠ 2.0!)
- [ ] Sem Sidecars no IDE
- [ ] Resto igual ao 2.0: MCP, Rules, Plugins, Hooks

## Módulo 8 — Firebase Studio Migration
- [ ] Por que migrar (controle local, agente real vs code completion)
- [ ] Pré-requisitos: Node v20+, Firebase CLI v15.10.0+
- [ ] Opção A (automática): Zip → IDE → @fbs-to-agy-export
- [ ] Opção B (manual): npx firebase-tools@latest studio:export
- [ ] Otimizado para: Next.js, Flutter, Angular
- [ ] Publicar: "Publish my app" no chat do agent
