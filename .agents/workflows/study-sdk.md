# Study SDK — Antigravity SDK Study Session

Start or resume a structured study session for the Antigravity SDK.

## Steps

1. Read `sdk/STUDY_PLAN.md` and identify the first unchecked module (`[ ]`).

2. If all modules are checked, congratulate the user and suggest reviewing `meta/surface-comparison.md` or starting another surface.

3. For the current unchecked module:
   - Read the corresponding section in `sdk/mastering-guide.md`
   - Teach the concept: explain the "why" first, then the "how"
   - For policy/hooks modules: show working Python code examples
   - Explain how SDK concepts map to their equivalents in CLI/2.0 when relevant
   - Ask 1-2 verification questions

4. After the user confirms understanding:
   - Mark the module as `[x]` in `sdk/STUDY_PLAN.md`
   - Ask: "Quer continuar com o próximo módulo ou encerrar por hoje?"

5. If continuing, repeat from step 1.

## Context

- Source of truth: `sdk/mastering-guide.md`
- Progress tracker: `sdk/STUDY_PLAN.md`
- Quick lookup: `sdk/quick-reference.md`
- Install: `pip install google-antigravity`
