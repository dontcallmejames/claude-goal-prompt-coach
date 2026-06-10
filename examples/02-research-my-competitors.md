# Example 2 — "research my competitors"

A research idea where "done" is fuzzy. The coach converts an open-ended task
into a verifiable deliverable so the loop can actually terminate.

## Input

```
/goal-prompt-coach:goal-coach do a deep dive on my competitors
```

## Turn 1 — coach asks

**Your idea, restated:** Produce a competitive research deep-dive you can act on.

**My assumptions (correct any):**
- The deliverable is a written report (a Markdown file), not a live dashboard.
- You want this saved to the repo/working dir, not just printed to chat.
- "Competitors" is a set you'll name, not for me to guess.
- Public information only (sites, pricing pages, docs, reviews) — no paid data.

**Questions — defaults in brackets:**
1. **Who are the competitors?** Name them, or give me the criteria to pick them.
   [you must name 3–7]
2. **What decision does this inform?** Pricing, positioning, feature gaps?
   [feature + pricing gaps vs. your product]
3. **What proves it's done?** [a competitors.md report covering every named
   competitor across a fixed set of dimensions, each claim with a source URL]
4. **Dimensions to compare?** [pricing, core features, target user,
   strengths, weaknesses]
5. **Off-limits?** [no paid/gated sources; don't fabricate numbers — cite or
   mark "unknown"]

## Your answers

> Competitors: Linear, Jira, Height. Inform our positioning vs them. Compare
> pricing, core features, target user, and one standout strength + weakness
> each. Markdown report. Cite sources, mark anything you can't verify as
> unknown. Defaults otherwise.

## Turn 2 — coach delivers

```text
/goal Produce competitors.md — a sourced competitive deep-dive on Linear, Jira, and Height — to inform our product positioning.

## Goal
Write a Markdown report at ./competitors.md comparing Linear, Jira, and Height across a fixed set of dimensions, with every factual claim backed by a public source URL, to inform our positioning.

## Success condition (verify before stopping)
Met ONLY when all are objectively true:
1. ./competitors.md exists and has one section per competitor: Linear, Jira, Height (all three present).
2. Each competitor section covers all of: pricing, core features, target user, one standout strength, one notable weakness.
3. Every factual claim (price, feature, user count, etc.) has an inline source URL, OR is explicitly marked "unknown — unverified". No unsourced facts.
4. A final "Positioning implications" section lists at least 3 concrete takeaways for our product, each tied to a gap found above.
5. Spot-check passes: pick 3 cited claims at random, open the URLs, confirm each page actually supports the claim.

## Constraints
- Public sources only (sites, pricing/docs pages, reputable reviews). No paid or gated data.
- Do not fabricate numbers. If a fact can't be verified from a public source, mark it "unknown".
- Keep it scannable: tables for pricing/features, prose for analysis.

## Iteration policy
- Research one competitor fully (all dimensions + sources) before moving to the next.
- After drafting, run the spot-check in criterion 5; if any cited page doesn't support its claim, fix the claim or its source and re-check.
- Fix and re-run until all 5 criteria pass.

## Stop conditions
- DONE: competitors.md complete, all claims sourced or marked unknown, spot-check passed. Report the file path and the 3 strongest positioning takeaways.
- BLOCKED (stop + ask): if a competitor's pricing or core info is entirely behind a gate/login and no public source exists. Note which competitor and dimension, and proceed with the rest marked "unknown".
```

**Handoff:** The likely BLOCKED trigger is gated pricing with no public source —
the prompt tells the agent to mark it unknown and keep going rather than stall.
Paste it into a new `/goal`.

Note the conversion: "deep dive on competitors" (never-ending) became **"a
competitors.md with N named competitors × fixed dimensions, every claim
sourced, spot-check passes"** — a condition that can actually be reached.

## v1.1.0 — updated coach, live run (before/after)

Same idea and answers fed to the v1.1.0 coach (live, via
`claude --plugin-dir`). The emitted prompt (1,874 chars) kept all five
sections:

```text
/goal Produce a competitor analysis markdown report on Linear, Jira, and Height to inform product positioning.

## Goal
A file `competitor-analysis.md` comparing Linear, Jira, and Height across
pricing, core features, target user profile, one standout strength, and one
standout weakness per competitor — plus a Positioning Implications section
drawing at least 3 differentiation observations.

## Success condition (verify before stopping)
Met ONLY when all are objectively true:
1. `competitor-analysis.md` exists in the working directory.
2. Each competitor section covers all five dimensions: pricing tiers/price
   points, core feature set, target user profile, one standout strength,
   one standout weakness.
3. Every factual claim includes an inline source URL, or is explicitly
   marked "unknown."
4. A "Positioning Implications" section exists with at least 3 actionable
   observations.
5. No placeholder or "TBD" remains in the file.

## Constraints
- Fetch pricing and features from each product's official site; do not rely
  solely on third-party summaries.
- If a data point cannot be verified from a primary source, mark it
  "unknown" — never extrapolate or assume.
- Single markdown file output only.

## Iteration policy
- Fetch each competitor's pricing page and feature overview before drafting
  that section; don't write from memory.
- When you have enough information to act, act — don't re-survey sources
  already read.
- After drafting, scan the file to confirm every claim is cited or marked
  "unknown" before finalizing.

## Stop conditions
- DONE: all five criteria confirmed by reading the file. Report the path and
  list any "unknown" items so the user can follow up manually.
- BLOCKED (stop + ask): pricing requires login and no public data exists; or
  scope changes to add competitors. State the blocker and your recommended
  option.
```

**Fable-traceable differences vs v1.0.0** (citations in fable-5.md):

- **Anti-overplanning in iteration policy** (§5): "When you have enough
  information to act, act — don't re-survey sources already read."
- **Primary-source grounding with explicit unknowns flagged in the DONE
  report** (§1): the agent must list its "unknown" items rather than papering
  over gaps — the report carries its own evidence audit.
- **BLOCKED reduced to input-only-the-user-can-give and scope change** (§6).
- **Leaner phrasing throughout** (§3): no spot-check enumeration; one
  verifiable citation rule does the work of v1.0.0's longer checklist.
