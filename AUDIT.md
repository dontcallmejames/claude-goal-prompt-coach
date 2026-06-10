# Fable 5 optimization & backward-compatibility audit

Date: 2026-06-10
Scope: plugin v1.1.0, repo working tree (source of truth) vs installed cache at
`~/.claude/plugins/cache/dontcallmejames/goal-prompt-coach/1.1.0/`.
Result: **0 unresolved findings. No file changes required; no version bump.**

## 1. Cache sync check

Every cache file was hash-compared against the repo. All six plugin files
differ by hash but `git diff --no-index --ignore-all-space` shows **zero
content differences** — the delta is line endings only (repo CRLF, cache LF,
a normal install-time artifact). The cache's extra `.in_use/` files are
runtime lock files, not plugin content. **In sync.**

## 2. Citation re-verification (re-fetched 2026-06-10)

| Source | Fetch result |
| --- | --- |
| [F5-PROMPT] Prompting Claude Fable 5 | Reachable. All cited sections present by exact name: "Ground progress claims during long runs", "Recommended scaffolding changes", "Strong instruction following", "Longer turns by default", "Construct a memory system", "Rare cases of early stopping". |
| [F5-INTRO] Introducing Claude Fable 5 and Claude Mythos 5 | Reachable. Covers the API-level items fable-5.md deliberately excludes (adaptive thinking always-on, effort, `stop_reason: "refusal"`, fallbacks) — confirming the "Explicitly NOT encoded" rationale. |
| [BEST] Prompting best practices | Reachable. Contains the dial-back-aggressive-language guidance ("Where you might have said 'CRITICAL: You MUST use this tool when...', you can use more normal prompting"), supporting fable-5.md item 3's secondary citation. |

All nine quoted claims in fable-5.md were confirmed verbatim or
near-verbatim (ellipsis-truncated) on the live pages. No corrections needed.

## 3. Guidance item → implementation map

| fable-5.md item | Implemented in |
| --- | --- |
| 1. Evidence-grounded progress claims | SKILL.md Turn 2 step 4, first bullet ("evidence-backed claims... unverified items flagged"); goal-loop.md "Evidence-grounded DONE reports" |
| 2. Fresh-context verifier subagents | SKILL.md Turn 2 step 4, third bullet; goal-loop.md "Verifier subagents in the iteration policy" |
| 3. Brevity over enumeration / no emphatic repetition | SKILL.md Turn 2 step 4, fourth bullet; goal-loop.md "Brevity over enumeration" |
| 4. Never request reasoning echo | SKILL.md Turn 2 step 4, fifth bullet; goal-loop.md "Never request reasoning echo" |
| 5. Act once informed | SKILL.md Turn 2 step 4, third bullet ("act once informed, don't overplan"); goal-loop.md "Act once informed" |
| 6. BLOCKED = three categories only | SKILL.md Turn 2 step 4, second bullet; goal-loop.md "BLOCKED = exactly three things" |
| 7. Notes file for multi-session goals | SKILL.md Turn 1 Scale question (span-sessions probe) + Turn 2 step 4, third bullet; goal-loop.md "A notes file for multi-session goals" |
| 8. Aim higher / don't shrink scope | SKILL.md Turn 1 Scale question; goal-loop.md "Good fits for /goal" |
| 9. Long-turn expectations (context only) | Referenced in goal-loop.md "Good fits for /goal" (§8–9); correctly carries no separate change |

No gaps. Every item maps to populated skill text.

## 4. Backward-compatibility check

- No instruction in SKILL.md, goal-coach.md, or the references hard-requires
  Fable 5. Fable mentions are rationale ("degrades output on Fable 5"), never
  preconditions.
- Both reference files state the additive framing explicitly: goal-loop.md
  ("plain prompt instructions — additive, and safe on any current Claude
  model") and fable-5.md ("Nothing here hard-requires Fable 5; on other
  models these instructions are simply good hygiene").
- The emitted-prompt template contains no model names and no model-gated
  sections. **Pass.**

## 5. Anti-pattern scan

Repo-wide grep for "CRITICAL", "you MUST", "show your thinking", "explain
your reasoning", reasoning-echo phrasing: every hit (SKILL.md:102–104,
goal-loop.md:93/99, fable-5.md:51/55/62–63) is the skill *prohibiting* the
pattern, not using it. Neither the skill files nor the template they emit
instruct an agent to echo reasoning or use emphatic-repetition emphasis.
**Pass.**

## 6. Live test

A fresh-context subagent followed SKILL.md against the sample idea "make my
Flask app's test suite faster" (crisp idea + "use your defaults", per the
Rules' compressed-Turn-1 path). Output measured:

- /goal prompt content: **2,463 characters** (< 4,000 limit)
- All five sections populated (Goal, Success condition, Constraints,
  Iteration policy, Stop conditions)
- DONE grounded in evidence ("only claims tied to an actual tool result,
  with anything unverified flagged as such")
- BLOCKED limited to the documented categories (input only the user can
  provide; real scope change)

**Pass.**

## Disposition

Zero findings required fixes, so per the audit's own rule no version bump is
made; this AUDIT.md is the only new file committed. The two-turn coaching
flow was audited as-is and not redesigned.
