# SDK Study Plan

> Progresso: 0/7 módulos concluídos

Invoke `/study-sdk` to start or resume your session. The agent reads from `sdk/mastering-guide.md`.

---

## Módulo 1 — O que é o SDK
- [ ] Framework Python para agentes autônomos programáticos
- [ ] Mesmo core agent harness do CLI e 2.0
- [ ] Quando usar SDK vs outros surfaces (tabela de decisão)
- [ ] Install: pip install google-antigravity

## Módulo 2 — Quick Start & Hello World
- [ ] Agent + LocalAgentConfig: padrão async context manager
- [ ] agent.chat() vs agent.stream()
- [ ] response.text(): awaitable
- [ ] Rodar o hello world e entender o output

## Módulo 3 — Core Pillar 1: Governed Extensibility
- [ ] Built-in tools (file I/O, shell, search)
- [ ] Custom Python functions como tools
- [ ] MCP Servers via SDK (stdio/SSE/HTTP)
- [ ] Agent Skills (mesmo SKILL.md format)

## Módulo 4 — Core Pillar 2: Safety Policies
- [ ] deny() / allow() / ask_user() API
- [ ] Padrão "deny all, allow specific"
- [ ] Handler function para ask_user (human-in-the-loop)
- [ ] Equivalência com o sistema deny/ask/allow do CLI

## Módulo 5 — Core Pillar 3: Lifecycle Hooks
- [ ] 3 categorias: Inspect (non-blocking), Decide (blocking), Transform (modifying)
- [ ] 9 lifecycle points (session start → session end)
- [ ] Hook(on=..., handler=..., category=...) API
- [ ] Exemplo: logger + safety gate

## Módulo 6 — Key Capabilities
- [ ] Streaming: agent.stream() → async iterator de chunks
- [ ] Multimodal: from_file() para imagens, PDFs, áudio, vídeo
- [ ] Structured Output: Pydantic models → output tipado e validado
- [ ] Observability: response.usage + response.thinking_traces

## Módulo 7 — Sub-agents & Human-in-the-Loop
- [ ] Sub-agents via SDK: agents filhos com tools independentes
- [ ] Human-in-the-Loop: pausar execução para input estruturado
- [ ] Quando usar SDK multi-agent vs CLI /teamwork-preview
- [ ] Integração com CI/CD: -p headless (CLI) vs SDK nativo
