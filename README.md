# Antigravity Study Vault

> Base de conhecimento completa do Antigravity, compilada de 74 páginas de documentação oficial + changelog completo (v1.0.0–1.0.6 CLI / v1.11.2–2.0.11 IDE/2.0).

---

## Como iniciar uma sessão de estudo

```bash
cd ~/Projects/agy-study
agy
```

O agente carrega automaticamente o `tutor-mode` (rule Always On) e está pronto para ensinar.

---

## Slash commands de estudo

| Comando | Surface | Guide fonte | Checklist |
|---|---|---|---|
| `/study-cli` | Antigravity CLI | `cli/mastering-guide.md` | `cli/STUDY_PLAN.md` |
| `/study-2.0` | Antigravity 2.0 | `2.0/mastering-guide.md` | `2.0/STUDY_PLAN.md` |
| `/study-ide` | Antigravity IDE | `ide/mastering-guide.md` | `ide/STUDY_PLAN.md` |
| `/study-sdk` | Antigravity SDK | `sdk/mastering-guide.md` | `sdk/STUDY_PLAN.md` |

---

## Protocolo de cada sessão

Quando você invoca `/study-xxx`, o agente:

1. **Lê o STUDY_PLAN.md** da surface escolhida
2. **Encontra o primeiro módulo não concluído** (`[ ]`)
3. **Ensina aquele módulo** — conceito + explicação + exemplo prático executável
4. **Faz 1-2 perguntas de verificação** antes de avançar
5. **Marca `[x]`** no STUDY_PLAN.md quando você confirmar que entendeu
6. **Pergunta se quer continuar** ou encerrar a sessão

Para continuar de onde parou em outra sessão, basta invocar o mesmo `/study-xxx` — o agente retoma pelo próximo `[ ]` automaticamente.

---

## Comandos úteis durante o estudo

| O que dizer | Efeito |
|---|---|
| `próximo` | Avança para o próximo módulo |
| `repete` / `não entendi` | Agente reexplica com outra abordagem |
| `exemplo prático` | Agente gera um exercício executável no terminal |
| `me testa` | Agente faz uma mini-avaliação do módulo atual |
| `pula` | Pula o módulo atual (não marca como concluído) |
| `status` | Mostra progresso atual de todos os STUDY_PLANs |
| `surface-comparison` | Agente consulta `meta/surface-comparison.md` |

---

## Mapa completo de arquivos

```
~/Projects/agy-study/
│
├── README.md                          ← você está aqui
│
├── .agents/
│   ├── rules/
│   │   └── tutor-mode.md              ← Always On: protocolo de ensino
│   └── workflows/
│       ├── study-cli.md               → gera /study-cli
│       ├── study-2.0.md               → gera /study-2.0
│       ├── study-ide.md               → gera /study-ide
│       └── study-sdk.md               → gera /study-sdk
│
├── cli/
│   ├── mastering-guide.md             ← 18 seções, SSOT canônico do CLI
│   ├── quick-reference.md             ← 7 tabelas compactas para consulta rápida
│   └── STUDY_PLAN.md                  ← 14 módulos com checkboxes
│
├── 2.0/
│   ├── mastering-guide.md             ← 11 seções
│   ├── quick-reference.md
│   └── STUDY_PLAN.md                  ← 10 módulos com checkboxes
│
├── ide/
│   ├── mastering-guide.md             ← 11 seções + Firebase migration
│   ├── quick-reference.md
│   └── STUDY_PLAN.md                  ← 8 módulos com checkboxes
│
├── sdk/
│   ├── mastering-guide.md             ← 9 seções, 3 pillars + 6 capabilities
│   ├── quick-reference.md
│   └── STUDY_PLAN.md                  ← 7 módulos com checkboxes
│
└── meta/
    └── surface-comparison.md          ← 9 tabelas comparativas: quando usar cada surface
```

---

## Ordem de estudo recomendada

### Prioridade 1 — CLI (foco atual)
```
/study-cli  →  14 módulos  →  ~7-10 sessões
```

### Prioridade 2 — Surface Comparison (quando terminar CLI)
```
Ler: meta/surface-comparison.md
```

### Prioridade 3 — 2.0, IDE, SDK (quando e se precisar)
```
/study-2.0   →  10 módulos
/study-ide   →  8 módulos
/study-sdk   →  7 módulos
```

---

## Notas importantes

- **Persona do agente:** O `tutor-mode` é uma camada adicional sobre sua persona global (user_global). Não substitui nada — é um chapéu extra para o contexto de estudo.
- **Fonte de verdade:** Todos os guides foram compilados de 74 páginas de documentação oficial + análise completa do changelog. Paths e discrepâncias foram verificados no sistema de arquivos real.
- **MCP path canônico:** `~/.gemini/config/mcp_config.json` (migrado em CLI v1.0.3) — não o path legacy em `antigravity-cli/`.
- **AGENTS.md:** Aceito como alternativa ao GEMINI.md desde v1.20.5.
