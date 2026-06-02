# The /goal autonomous-loop format

`/goal` puts Claude Code into an autonomous loop: it works, checks whether the
goal condition is met, and keeps going **without checking in** until the
condition holds. A session-scoped Stop hook blocks stopping until the condition
is satisfied, then auto-clears.

The single idea that makes or breaks a goal prompt:

> **Reliability comes from the harness, not the model.** A well-structured goal
> with an evaluable success condition beats a cleverly-worded vague one. Most
> people misuse `/goal` by writing conditions the agent cannot actually check.

## The three failure modes a goal prompt must design against

1. **Halting for confirmation.** The agent stops to ask "should I proceed?"
   instead of continuing. → Fix: state the iteration policy and tell it to keep
   going; reserve stopping for genuinely blocked states.
2. **Infinite loops.** The agent never decides it is done, or thrashes. → Fix:
   an *evaluable* success condition plus explicit DONE criteria.
3. **Plausible-but-wrong output.** The agent declares victory on something that
   looks right but isn't verified. → Fix: a success condition the agent verifies
   by observation (run the test, read the file, measure the value) before
   stopping.

## Required sections of a /goal prompt

A strong `/goal` prompt has these sections, all populated — no placeholders:

- **Goal** — one or two sentences naming the concrete artifact/outcome.
- **Success condition (verify before stopping)** — a numbered list of
  objectively checkable conditions. This is the heart of the prompt.
- **Constraints** — what to follow, avoid, or not touch (stack, scope, files,
  "don't guess X — verify it").
- **Iteration policy** — how to self-test and what to do on partial results;
  "fix and re-run until all criteria pass."
- **Stop conditions** — **DONE** (all criteria verified, what to report) and
  **BLOCKED** (the narrow set of situations where it should stop and ask, plus
  "state the blocker and your recommended option").

## What makes a success condition evaluable

Each condition must be something the agent can confirm by *doing*, not by
*believing*. The test: "Could two people disagree about whether this is met?"
If yes, it is not evaluable yet — rewrite it.

| Vague (reject) | Evaluable (rewrite to) |
| --- | --- |
| "Make the app faster" | "Largest Contentful Paint on `/` is under 2.0s measured by Lighthouse, down from the current 4.1s" |
| "Add tests" | "`npm test` runs and all tests pass; coverage report shows the `auth/` module at or above 80%" |
| "Clean up the code" | "`npm run lint` exits 0 with no warnings; no function exceeds 50 lines (checked by the linter rule)" |
| "Make it good" | "The 6 acceptance checks in the checklist below each pass when run" |
| "Document it" | "`README.md` exists, contains install + usage sections, and the example commands in it run without error" |

Heuristics the coach uses to harden a condition:

- Bind it to a **command, file, or number**: a test name, an exit code, a file
  path that must exist, a measured threshold with a unit.
- Prefer **before → after** numbers for performance/quality goals.
- Make **"done" countable**: "all N checks pass," not "it works."
- Push verification into the loop: "verify by running X" so the agent checks
  itself rather than assuming.
- If a goal is inherently subjective, convert it to a **proxy that is
  checkable** (a passing acceptance checklist, a golden-file diff, a rubric the
  agent scores against and reports).

## Good fits for /goal

Long-horizon, verifiable work: code-intensive builds (dashboards, backtests),
research deep-dives with a defined deliverable, migrations, audits — anything
where "done" can be stated as a condition the agent can check.
