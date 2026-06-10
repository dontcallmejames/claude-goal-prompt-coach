# Fable 5 research notes (v1.1.0)

Every Fable-5-specific behavior encoded in this plugin traces to one of these
fetched Anthropic sources. If a claim isn't here with a citation, the plugin
must not make it.

Sources (fetched 2026-06-09):

- **[F5-PROMPT]** Prompting Claude Fable 5 —
  <https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5>
- **[F5-INTRO]** Introducing Claude Fable 5 and Claude Mythos 5 —
  <https://platform.claude.com/docs/en/about-claude/models/introducing-claude-fable-5-and-claude-mythos-5>
- **[BEST]** Prompting best practices —
  <https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices>

All items below are *prompt-level* guidance — plain instructions that are safe
and sensible on any current Claude model. Nothing here hard-requires Fable 5;
on other models these instructions are simply good hygiene. That is what keeps
v1.1.0 backward compatible.

## Guidance items and the plugin changes they justify

### 1. Ground progress claims in evidence [F5-PROMPT § "Ground progress claims during long runs"]

> "On long autonomous runs, instruct Claude Fable 5 to audit progress against
> actual tool results. In Anthropic's testing, this nearly eliminated
> fabricated status reports even on tasks designed to elicit them."

**Change:** The coach's emitted DONE stop condition now requires
evidence-backed reporting ("report only what you can point to a tool result
for"). Added to SKILL.md Turn 2 and goal-loop.md.

### 2. Fresh-context verifier subagents beat self-critique [F5-PROMPT § "Recommended scaffolding changes"]

> "Make self-verification explicit in long-run prompts. Separate,
> fresh-context verifier subagents tend to outperform self-critique."

**Change:** For long-horizon goals, the coach's emitted Iteration policy may
recommend periodic verification by a fresh-context subagent against the
success condition. Added to SKILL.md Turn 2 and goal-loop.md.

### 3. Brief instructions beat enumeration; over-prescription degrades output [F5-PROMPT § "Strong instruction following", § "Recommended scaffolding changes"]

> "Instruction-following is improved enough that you can steer most behaviors
> with a brief instruction rather than enumerating each behavior by name."
> "Skills developed for prior models are often too prescriptive for Claude
> Fable 5 and can degrade output quality."

**Change:** The coach now prefers one well-aimed sentence over exhaustive
lists when writing Constraints and Stop conditions, and avoids piling on
emphatic repetition ("CRITICAL: you MUST..."). Added to SKILL.md Turn 2.
[BEST] makes the same point about dialing back aggressive tool-trigger
language on recent models.

### 4. Never ask the agent to echo its reasoning [F5-PROMPT § "Recommended scaffolding changes"]

> "Prompts, skills, or harness instructions that tell the model to echo,
> transcribe, or explain its internal reasoning as response text can trigger
> the `reasoning_extraction` refusal category on Claude Fable 5, causing
> elevated fallbacks."

**Change:** The coach must not emit prompts containing "show your thinking /
explain your reasoning step by step in your response" style instructions, and
rewrites them into observable alternatives (report decisions and evidence,
not reasoning). Added to SKILL.md Turn 2 and goal-loop.md.

### 5. Act once informed; don't overplan [F5-PROMPT § "Longer turns by default"]

> Sample: "When you have enough information to act, act. Do not re-derive
> facts already established... If you are weighing a choice, give a
> recommendation, not an exhaustive survey."

**Change:** goal-loop.md's iteration-policy guidance now includes an
act-once-informed line for emitted prompts, countering the documented
overplanning tendency at higher effort.

### 6. Pause only where genuinely required [F5-PROMPT § "Strong instruction following", § "Rare cases of early stopping"]

> Sample: "Pause for the user only when the work genuinely requires them: a
> destructive or irreversible action, a real scope change, or input that only
> they can provide."

**Change:** This is the documented articulation of what BLOCKED should be.
goal-loop.md now states BLOCKED conditions should be limited to exactly these
three categories. (The v1.0.0 design already pointed this way; the citation
makes it a rule.)

### 7. Memory/notes for multi-session goals [F5-PROMPT § "Construct a memory system"]

> "Claude Fable 5 performs particularly well when it can record lessons from
> previous runs and reference them. Provide a place to write notes, as simple
> as a Markdown file."

**Change:** For goals likely to span sessions or run for hours/days, the
coach may add a progress/lessons file (e.g. `progress.md`) to the emitted
Iteration policy. Added to SKILL.md Turn 2 and goal-loop.md.

### 8. Aim higher [F5-PROMPT § "Recommended scaffolding changes", intro paragraph]

> "Start at the top of your difficulty range." "Claude Fable 5 takes on
> problems that were previously too complex, long-running, or ambiguous...
> testing it only on simpler workloads tends to undersell its capability
> range."

**Change:** In Turn 1, when an idea has an obvious more ambitious end-to-end
framing, the coach may offer it as one question ("scope it bigger?") rather
than silently shrinking the goal to what older models could handle.

### 9. Long-turn expectations [F5-PROMPT § "Longer turns by default"; F5-INTRO]

> "Individual requests on hard tasks can run for many minutes at higher
> effort settings... autonomous runs can extend for hours."

**Context only** — supports items 1, 2, and 7; no separate change.

## Explicitly NOT encoded (and why)

- **Effort/adaptive-thinking API parameters** [F5-INTRO] — API-level, not
  controllable from a /goal prompt; would also break model-agnosticism.
- **Safety classifiers / refusal stop reason / fallbacks** [F5-INTRO] —
  integration concerns for API builders, irrelevant to coaching a goal prompt.
- **send-to-user tool, summarized-only thinking output** [F5-PROMPT, F5-INTRO]
  — harness features, not prompt content.
- Anything about benchmarks or "Fable is smarter" in general — not actionable
  in a prompt.
