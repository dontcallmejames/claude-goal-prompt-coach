# Example 4 — "migrate my legacy jQuery site to React" (v1.1.0, live run)

A fresh multi-session idea, run live against the v1.1.0 coach. This is the
example that exercises the long-horizon (Fable 5) tuning most fully: a
progress/lessons file, fresh-context verifier subagents, evidence-grounded
reporting, and BLOCKED phrased as exactly the three documented pause
categories. Citations: `skills/goal-prompt-coach/references/fable-5.md`.

## Input

```
/goal-prompt-coach:goal-coach migrate my legacy jQuery site to React
```

## Answers given

> 40-page marketing + docs site, jQuery 2.x with custom plugins; target React
> 18 + Vite; pixel-similar visuals; will take multiple sessions over days;
> proof is existing Playwright visual snapshots passing plus all pages
> rendering; don't break SEO URLs; defaults otherwise.

## Coach output (live, 2,574 chars — under the 4,000 limit)

```text
/goal Migrate a 40-page jQuery 2.x marketing + docs site to React 18 + Vite, with pixel-similar visuals, all Playwright visual snapshots passing, and every original SEO URL preserved.

## Goal
A fully migrated React 18 + Vite codebase where every page that existed in the jQuery site renders in React with no JS errors, all URL paths unchanged, and visual output matching the existing Playwright snapshot baselines.

## Success condition (verify before stopping)
Met ONLY when all are objectively true:
1. `npm run build` exits 0 on the Vite project with no errors.
2. `npx playwright test` exits 0 with 0 snapshot failures across all 40 pages.
3. Every original URL path is served by a React route — verified by diffing the pre-migration route list (record it in `progress.md` at session start) against the live Vite dev-server routes.
4. `grep -r "jquery" src/` returns 0 matches (no jQuery or plugin imports remain in source).
5. Each page produces 0 uncaught JS console errors, verified by a Playwright console-error listener run across all pages.

## Constraints
- Stack: React 18 + Vite; no other framework changes.
- Rewrite custom jQuery plugins as React components or hooks — do not wrap them in jQuery.
- Do not alter any URL path or substitute redirects for real routes.
- Before starting, verify that the Playwright snapshot baseline exists and the pre-migration route list is recorded in `progress.md`; if either is missing, stop (BLOCKED).

## Iteration policy
- Work page-by-page. After each page is migrated, run that page's Playwright snapshot test and fix failures before moving on.
- Keep `progress.md` updated with pages completed, open issues, lessons, and next steps; update at the close of every working session so future sessions start with full context.
- At each 10-page milestone and at session end, spawn a fresh-context subagent to run the full snapshot suite and verify the success condition.
- When you have enough information to act, act — don't re-derive facts already confirmed or survey options you won't pursue.

## Stop conditions
- DONE: all 5 criteria verified by actual tool runs; report pages migrated, snapshot pass rate, and any criteria that could not be verified — flag unverified items explicitly.
- BLOCKED (stop + ask): a destructive or irreversible action (e.g., deleting original source); a real scope change (e.g., adding new pages or redesigning components); or input only the user can provide (e.g., missing assets, credentials, or plugin source absent from the repo). State the blocker and your recommended path forward.
```

## Fable-traceable features in this output

- **`progress.md` state/lessons file** (fable-5.md §7) — multi-session answer
  triggered it: route list recorded at start, state updated every session.
- **Fresh-context verifier subagents** (§2) — "at each 10-page milestone and
  at session end, spawn a fresh-context subagent to run the full snapshot
  suite." Verifier subagents outperform self-critique on long runs per
  Anthropic's guidance.
- **Evidence-grounded DONE** (§1) — "verified by actual tool runs... flag
  unverified items explicitly."
- **BLOCKED = the three documented pause categories, verbatim** (§6) —
  destructive/irreversible action, real scope change, input only the user can
  provide.
- **Act-once-informed** (§5) — "don't re-derive facts already confirmed or
  survey options you won't pursue."
- **Real end-to-end scope** (§8) — the coach kept the full 40-page,
  multi-day migration as one goal instead of shrinking it to a pilot batch,
  letting the success condition bound the work.
