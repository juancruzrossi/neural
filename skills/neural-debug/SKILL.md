---
name: neural-debug
description: "Root-cause investigation for bugs and failures"
---

# Neural Debug

**Iron Law: no fix without root cause. Do not patch symptoms.**

If the bug relates to an active feature in `.neural/wip/`, read its `CONTEXT.md`, `PLAN.md`, and ADRs first — intended behavior and binding decisions live there.

## Procedure

1. **Reproduce before reasoning.** Get the failure to happen on demand — a failing test, a script, a curl. Evidence first, no guessing at causes yet.

2. **Instrument, don't theorize.** For multi-component paths (API → service → DB), log what enters and exits each boundary and run once — let the logs say WHERE the data goes wrong. Tag every diagnostic log with a unique prefix (e.g. `[DEBUG-a4f2]`) so cleanup is a single grep.

3. **Trace backward from the failure.** Start at the exact failure point and ask "what called this with this value?" until you reach where the bad data originated — certainty toward unknown beats forward speculation.

4. **Hypothesize falsifiably.** 2-3 ranked hypotheses, each with the evidence that would confirm or refute it. Test them. If 3+ are refuted, or each fix reveals a new problem elsewhere, STOP — that pattern signals an architectural issue; report it instead of attempting more fixes.

5. **Fix the root cause, minimally.** Remove every `[DEBUG-…]` log. Re-run the reproduction — if the fix works but you can't explain *why*, that's luck, not a fix: find the mechanism before closing. Run the test suite; commit atomically if the repo uses git, describing cause and fix.

Then suggest: "Fixed. Run neural-review to verify the full feature still works."
