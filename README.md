# Goal Prompt Coach (for Claude Code)

Turn a vague idea into a strong, ready-to-paste Claude `/goal` prompt.

`/goal` runs Claude Code in an autonomous loop until a success condition is met.
The catch: **most goals fail because the success condition isn't something the
agent can actually check.** This plugin is a coach that interviews your rough
idea and hands back a `/goal` prompt with an *evaluable* success condition,
constraints, an iteration policy, and stop conditions — paste-ready.

It's the Claude Code counterpart to
[codex-goal-prompt-coach](https://github.com/dontcallmejames/codex-goal-prompt-coach).

## What you get

- **A command** — `/goal-prompt-coach:goal-coach <your idea>` — to summon the
  coach on demand.
- **A skill** — `goal-prompt-coach` — that Claude invokes automatically when you
  have a vague idea and ask for help phrasing it as a goal.

Both run the same **ask-then-deliver** flow: the coach surfaces its assumptions
and asks a few targeted questions, then emits the finished `/goal` prompt. It
**never builds the thing you described** — its only output is the prompt.

## Install

```shell
/plugin marketplace add dontcallmejames/claude-goal-prompt-coach
/plugin install goal-prompt-coach@dontcallmejames
```

That's it — two commands, installable from GitHub by anyone. Pin to a tag or
commit by adding a ref:

```shell
/plugin marketplace add dontcallmejames/claude-goal-prompt-coach@v1.0.0
```

Update later with `/plugin marketplace update dontcallmejames`.

> **Try before installing:** clone the repo and run
> `claude --plugin-dir ./plugins/goal-prompt-coach` to load it for one session.

## Use

Type your rough idea after the command:

```shell
/goal-prompt-coach:goal-coach make my app faster
```

The coach replies with its assumptions and 3–6 questions. Answer them (or say
"use your defaults"), and it returns a complete `/goal` prompt in a copyable
block. Paste that into a new `/goal` to run it.

You can also just describe a vague idea and ask Claude to "help me write a goal
prompt for this" — the skill triggers on its own.

> **Note on naming:** Claude Code namespaces plugin skills, so the command is
> invoked as `/goal-prompt-coach:goal-coach`. Type `/goal` and tab-complete to
> find it.

## What a good `/goal` prompt looks like

Every prompt the coach emits has five sections, all populated:

- **Goal** — the concrete outcome.
- **Success condition** — numbered, *evaluable* checks the agent verifies before
  stopping. The heart of the prompt.
- **Constraints** — scope, stack, what not to touch.
- **Iteration policy** — how to self-test and recover from partial results.
- **Stop conditions** — DONE (all checks verified) and BLOCKED (the narrow cases
  where it should stop and ask).

The coach's whole job is the second one: it rewrites "make it faster" into "LCP
under 2.5s, measured by Lighthouse, down from 4.1s" — a thing the agent can
actually check.

## Worked examples

- [Make my app faster](examples/01-make-my-app-faster.md) — a feeling becomes a
  measured threshold (with v1.0.0 → v1.1.0 before/after).
- [Research my competitors](examples/02-research-my-competitors.md) — an
  open-ended task becomes a verifiable deliverable (with before/after).
- [Proof run](examples/03-proof-run.md) — v1.0.0 end-to-end run on a fresh idea.
- [jQuery → React migration](examples/04-jquery-to-react.md) — v1.1.0 live run
  on a multi-session idea, showing the long-horizon tuning in full.

## What's new in 1.1.0 — tuned for Claude Fable 5

The coach now applies Anthropic's documented
[Fable 5 prompting guidance](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5),
with every claim cited in
[references/fable-5.md](plugins/goal-prompt-coach/skills/goal-prompt-coach/references/fable-5.md):
evidence-grounded DONE reports, fresh-context verifier subagents for long runs,
a `progress.md` state file for multi-session goals, BLOCKED limited to the
three documented pause categories, no reasoning-echo instructions (avoids the
`reasoning_extraction` refusal), and brevity over enumeration. All additive —
the plugin still works on any current Claude model.

## How it works

```
plugins/goal-prompt-coach/
├── .claude-plugin/plugin.json      plugin manifest
├── commands/goal-coach.md          the /goal-coach command (thin launcher)
└── skills/goal-prompt-coach/
    ├── SKILL.md                    auto-triggering coach procedure
    └── references/
        ├── goal-loop.md            /goal philosophy + evaluable-condition rules
        └── claude-prompting.md     Claude prompting best practices
```

Sources distilled into the knowledge base: Anthropic's
[prompt engineering best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
and the autonomous-loop `/goal` philosophy ("reliability comes from the harness,
not the model").

## License

MIT — see [LICENSE](LICENSE).
