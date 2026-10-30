---
name: subagent-model-economy
description: Use when work will be delegated to subagents - building a milestone or fix batch, reviewing a diff, running a study, or sweeping screens - and a model, a brief, or a review step has to be chosen before dispatch.
---

# Subagent model economy

The lead briefs, decides and verifies; subagents work on the cheapest model trusted for the job.
Nothing a subagent reports is done until the lead has checked it.

## Which model (always pass `model`)

| Work | Model |
|---|---|
| Build from a complete brief; plans, research | `sonnet` |
| Build code touching money, spend caps, the model-call path or frozen modules | `opus` |
| Independent read-only review of any diff | `opus` |
| Mechanical copying or formatting | `haiku` |
| The top-tier model (e.g. Fable) | never a subagent unless the user says so |

## The brief: a file, eight parts

The subagent gets only its path.

1. Repo, branch, "status clean first".
2. Hard rules, verbatim: kill by PID only; no stash, file checkout, hard reset or worktrees; no
   secret files; no deploy; no full test suite; no server left running.
3. Why: the user's words and the measured numbers.
4. What to build (a second view reuses the original's helpers and words), and what must not change.
5. The invariant most likely to break; its test written first and watched failing.
6. Mutation checks.
7. Exact commands.
8. Finish report: files, counts, mutations, errors in the brief.

## The loop

1. Builder in the background.
2. Full test run and reviewer in parallel. The reviewer gets the diff, the brief and the defect
   classes below, never your verdict.
3. Findings go to the same builder once the test run has released the checkout.
4. Rerun the key test and read the diff yourself; never trust "tests pass".
5. Merge, push, deploy with before/after reads; look at the running result (UI: screenshots at
   the user's real sizes).

## Defect classes reviewers hunt

- A cap or spend record that fails open: written after the paid call; unreadable read as
  "nothing spent"; an error treated as unbilled.
- A second view (phone, card) with its own words or arithmetic, or showing a figure the server
  declines to give.
- Fit-time features differing from decision time (stale prices).
- A "blind" model reaching the answer through a tool.
- A capability shipped "off" that still changes behaviour.

## Multi-agent workflows

- A claimed edge (backtest, cross-validated return): tell the user first it is unverified; then
  one skeptic per test - out of time, clustered by series or event, at prices that could really
  have been paid, corrected for every variant tried - then a judge. Nothing goes live and no
  stake grows before the verdict.
- Many screens: one reviewer per group, then a ranker who opens every screenshot.

## Common mistakes

- Reporting a finding before the skeptics.
- Two agents in one checkout (a hand-back is not proof it stopped).
- Resuming a builder during the test run.
