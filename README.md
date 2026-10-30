# wirb-model-economy

A Claude Code skill for delegating work to subagents: which model each subagent gets, the brief it
receives, and the review loop that checks its work before anything is called done.

> **Status: v0.1, not yet pressure-tested.** The skill was written from a working practice, but it
> has not yet been run through fresh-agent baseline and compliance tests (the
> superpowers `writing-skills` method). Use it, and open an issue if an agent finds a way around it.

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
