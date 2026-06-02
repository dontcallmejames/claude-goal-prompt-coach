# Example 1 — "make my app faster"

A vague idea with no evaluable target. Watch the coach force a measurable
condition before it will emit a prompt.

## Input

```
/goal-prompt-coach:goal-coach make my app faster
```

## Turn 1 — coach asks

**Your idea, restated:** Improve the runtime performance of your app so it feels
noticeably faster to users.

**My assumptions (correct any):**
- It's a web app (you mentioned "app" with no platform).
- "Faster" means page load / interaction latency, not build time.
- There's an existing codebase to optimize, not a rewrite.
- You can run it locally to measure before/after.

**A few questions — defaults in brackets, reply "defaults" to take them all:**
1. **How do we prove it's faster?** What's the metric and current number?
   [Lighthouse Largest Contentful Paint on the home route; you tell me the
   baseline]
2. **Which surface?** A specific slow page/route, or the app overall? [the
   slowest route]
3. **Target?** A number to beat, or "best effort within scope"? [LCP under 2.5s,
   or a 30% improvement, whichever you prefer]
4. **Off-limits?** Anything I shouldn't touch — framework, API contracts, the
   build pipeline? [don't change public API or swap frameworks]
5. **Stack?** [you tell me — assume Next.js if unspecified]

## Your answers

> Next.js. The /dashboard route is the slow one, LCP is about 4.1s in
> Lighthouse. Get it under 2.5s. Don't change the API or swap frameworks.
> Defaults for the rest.

## Turn 2 — coach delivers

```text
/goal Cut the Largest Contentful Paint of the Next.js /dashboard route from ~4.1s to under 2.5s without changing the public API or the framework.

## Goal
Optimize the /dashboard route of this Next.js app so it loads materially faster, measured by Lighthouse LCP, while leaving behavior and the public API unchanged.

## Success condition (verify before stopping)
Met ONLY when all are objectively true:
1. Lighthouse (mobile preset, production build, cold cache) reports LCP for /dashboard under 2.5s, down from the ~4.1s baseline. Capture both numbers.
2. The route renders the same data and UI as before — no visual regression in a before/after screenshot of /dashboard.
3. `npm run build` succeeds and `npm test` passes with no new failures.
4. No change to public API routes or response shapes (diff shows files under app/api/** unchanged in contract).
5. The framework is unchanged (still Next.js; package.json "next" version not swapped).

## Constraints
- Do not change the public API or any response shape.
- Do not swap frameworks or major libraries; optimize within the current stack.
- Prefer standard Next.js levers (server components, streaming, image/font optimization, code-splitting, caching) over custom infrastructure.
- Keep changes scoped to the /dashboard render path.

## Iteration policy
- Measure LCP before any change to lock the baseline, then after each change.
- Apply one optimization at a time; re-measure; keep what helps, revert what doesn't.
- Fix and re-run until LCP is under 2.5s and build + tests pass.

## Stop conditions
- DONE: LCP under 2.5s verified by a fresh Lighthouse run, build + tests green, no visual/API regression. Report before/after LCP and the changes that moved it.
- BLOCKED (stop + ask): if hitting 2.5s is impossible without changing the API or framework, or requires a backend/data change outside this repo. State the blocker, the best LCP achieved, and your recommended next step.
```

**Handoff:** The most likely BLOCKED trigger is the target being unreachable
without a backend/data change — the prompt tells the agent to surface that with
the best number it got rather than silently fall short. Paste it into a new
`/goal`.

Note how "make my app faster" became **"LCP under 2.5s from 4.1s, verified by
Lighthouse"** — a number the agent can measure, not a feeling.
