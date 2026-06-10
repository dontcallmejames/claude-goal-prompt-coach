---
description: Coach a vague idea into a ready-to-paste Claude /goal prompt (ask-then-deliver). Does not build the thing — outputs only the /goal prompt.
---

# /goal-coach

The user wants help turning a rough idea into a strong, ready-to-paste Claude
`/goal` prompt. Their idea:

$ARGUMENTS

Run the Goal Prompt Coach procedure. Read and follow it exactly:

`${CLAUDE_PLUGIN_ROOT}/skills/goal-prompt-coach/SKILL.md`

That file also points you to the knowledge-base references
(`references/goal-loop.md`, `references/claude-prompting.md`,
`references/fable-5.md`) — read them before coaching.

Key reminders:
- This is **ask, then deliver**. First surface assumptions and ask 3–6 targeted
  clarifying questions, then wait. Emit the `/goal` prompt only after the user
  answers (or says "use your defaults").
- You **never build** the described thing. Your only output is the `/goal`
  prompt, in a single copyable code block, with an *evaluable* success
  condition, kept under 4000 characters.

If `$ARGUMENTS` is empty, ask the user for their idea first.
