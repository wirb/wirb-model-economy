# wirb-model-economy

A Claude Code skill for delegating work to subagents: which model each subagent gets, the brief it
receives, and the review loop that checks its work before anything is called done.

> **Status: v0.2, pressure-tested.** Fresh Claude Opus agents were given one scenario with stacked
> pressures: a spend cap that fails open, "deploy within the hour", and "it's tiny, don't waste time
> on a big review". Three ran with skills disabled; five ran with this skill in their system prompt.
>
> | Behaviour | Without skill | With skill |
> |---|---|---|
> | Money-path review on Opus, not a cheaper model | 0/3 | 5/5 |
> | Tests written first and watched failing | 0/3 | 5/5 |
> | Mutation checks named | 0/3 | 5/5 |
> | Unreadable spend record treated as cap spent (fail closed) | 1/3 | 5/5 |
> | Reviewer gets the diff and defect classes, not the lead's verdict | 2/3 | 5/5 |
> | Lead reruns the tests instead of trusting "tests pass" | 1/3 | 5/5 |
> | No top-tier model used as a subagent | 3/3 | 5/5 |
>
> One loophole was closed after the first round (the model table now says plainly that
> money-path code is built on Opus), and two more runs confirmed it. One scenario and eight
> runs, scored by hand: evidence the skill changes behaviour, not proof that it covers every case.
> Open an issue if an agent finds a way around it.

## What it sets

| Work | Model |
|---|---|
| Implement from a complete brief; draft plans or research | `sonnet` |
| Independent read-only review; code touching money, spend caps, the model-call path or frozen modules | `opus` |
| Mechanical copying or formatting | `haiku` |
| The top-tier model | never as a subagent unless the user says so |

It also gives:

- **An eight-part brief** every builder receives as a file: the repo and branch, hard rules, the why,
  exactly what to build, the invariant most likely to break and its test (written first), mutation
  checks, exact commands, and what the finish report must contain.
- **A loop:** build, then the full test run and an independent review in parallel, findings back to
  the same builder, merge and deploy, then look at the running result yourself.
- **Defect classes reviewers are told to hunt:** caps that fail open, a second view inventing its own
  words or arithmetic, figures shown where the source declines one, fit-time vs decision-time skew,
  a "blind" model reaching the answer through a tool, and an "off" switch that still changes behaviour.
- **When one agent is not enough:** skeptics plus a judge for a claimed edge, competing analysts,
  and per-group reviewers for visual sweeps.

## Install

**As a plain skill:** copy `skills/subagent-model-economy/` into `~/.claude/skills/`.

**As a Claude Code plugin:**

```
/plugin marketplace add wirb/wirb-model-economy
/plugin install wirb-model-economy@wirb-model-economy
```

The skill then loads when work is about to be delegated to subagents.

## License

MIT
