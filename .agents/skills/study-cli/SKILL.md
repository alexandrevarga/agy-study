---
name: study-cli
description: Start or resume a structured study session for the Antigravity CLI.
---

# Study CLI — Antigravity CLI Study Session

Start or resume a structured study session for the Antigravity CLI.

## Steps

1. Read `cli/STUDY_PLAN.md` and identify the first unchecked module (`[ ]`).

2. If all modules are checked, congratulate the user and suggest reviewing `meta/surface-comparison.md` or starting another surface.

3. For the current unchecked module:
   - Read the corresponding section in `cli/mastering-guide.md`
   - Teach the concept: explain the "why" first, then the "how"
   - Provide a practical, terminal-executable example
   - Ask 1-2 verification questions

4. After the user confirms understanding:
   - Mark the module as `[x]` in `cli/STUDY_PLAN.md`
   - Ask: "Quer continuar com o próximo módulo ou encerrar por hoje?"

5. If continuing, repeat from step 1.

## Context

- Source of truth: `cli/mastering-guide.md`
- Progress tracker: `cli/STUDY_PLAN.md`
- Quick lookup: `cli/quick-reference.md`
- Current CLI version: v1.0.6
