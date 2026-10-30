# wirb-model-economy

A Claude Code skill for delegating work to subagents: which model each subagent gets, the brief it
receives, and the review loop that checks its work before anything is called done.

> **Status: v0.4, pressure-tested on Claude Opus and Claude Fable; 470 words, inside the
> superpowers `writing-skills` budget.** Fresh agents (skills switched
> off, no project files, no tools) were given scenarios that stack pressures against the skill. One
> arm ran without the skill, the other with it in the system prompt.
>
> **Round 2, Fable, three scenarios.** Each answer was scored against a fixed rubric by a separate
> Opus judge that was not told which arm it was reading; judgements were spot-checked by hand.
>
> | Scenario | Pressures | Without skill | With skill |
> |---|---|---|---|
> | Spend-cap bug that fails open (7 criteria) | "deploy within the hour", "tiny, skip the big review" | 4/7, 3/7 | 7/7, 7/7 |
> | Phone view of money figures (8 criteria) | "tonight", "compute a nicer figure in the frontend" | 6/8, 4/8 | 8/8, 8/8 |
> | Claimed +17-22% backtest edge (6 criteria) | "turn it on tomorrow and double the stake" | 5/6, 5/6 | 6/6, 4/6, then 6/6, 6/6 after the fix below |
>
> **Round 1, Opus, the spend-cap scenario, scored by hand:** money-path review on Opus 0/3 without
> vs 5/5 with; tests written first 0/3 vs 5/5; mutation checks 0/3 vs 5/5; unreadable spend record
> treated as cap spent 1/3 vs 5/5.
>
> **Loopholes closed:** v0.2 made explicit that money-path code is *built* on Opus. v0.3 made the
> claimed-edge line a recipe (tell the user it is unverified, then skeptics for out-of-time,
> clustering, executable prices and multiple comparisons, then a judge, nothing live and no bigger
> stake before the verdict) after one Fable run tested only two things and missed stale prices.
>
> **v0.4 trim:** the skill was cut from 644 to 470 words (body) and re-tested on Fable: spend cap
> 7/7, claimed edge 6/6, phone view 7/8 on the first run (the trim had dropped "reuse the original
> view's words" from the brief), then 8/8 and 8/8 once that clause was restored.
>
> **Limits:** 27 runs over three scenarios the author wrote, scored by an LLM judge. It shows the
> skill changes behaviour in the ways it intends; it does not prove coverage of every case. Open an
> issue if an agent finds a way around it.

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
