# Claude prompting best practices (applied to /goal prompts)

Distilled from Anthropic's prompting guidance, focused on what makes a `/goal`
prompt reliable.

## Be explicit

Claude calibrates effort and output to what you actually say. State the desired
end state, the format, and the boundaries directly. Don't imply — name it. A
goal prompt should read as a spec, not a wish.

- Say what to do *and* what "done" looks like.
- Name the output format the agent must produce.
- State constraints as constraints, not hopes ("do not modify the public API",
  "do not guess the schema — verify it against an installed example").

## Give context

Tell the agent why and against what. Reference the real stack, the existing
files, the prior art. A goal that points at "my Codex version" or "the current
4.1s LCP" gives the agent something concrete to align to and measure against.

## Define the output format

When the deliverable is itself a document (like a `/goal` prompt), specify its
exact sections and that they must be *populated, not placeholders*. Show the
shape you want. Ambiguity about format is where plausible-but-wrong output
creeps in.

## Use structure

Organize prompts with clear headings/sections (and XML tags when nesting
matters). A predictable structure — Goal, Success condition, Constraints,
Iteration policy, Stop conditions — makes the prompt easy for the agent to
follow and easy for a human to audit.

## Prefer evaluable instructions

The most important transfer from prompting practice to `/goal`: instructions the
agent can verify beat instructions it can only interpret. "All tests pass" is
checkable; "high quality" is not. Every requirement should be phrased so the
agent can confirm it by observation. See [goal-loop.md](goal-loop.md).

## Let it think, then verify

For non-trivial work, allow reasoning before acting, and require a verification
pass before declaring done. In a goal loop this is the iteration policy:
self-test, compare against the success condition, fix, re-run.

## Calibrate verbosity deliberately

Claude's latest models size their response to perceived task complexity. If you
need a specific length or density, say so. For the coach's *own* output, the
`/goal` prompt should be complete but tight — every line load-bearing.
