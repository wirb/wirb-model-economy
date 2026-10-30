---
name: subagent-model-economy
description: Use when work will be delegated to subagents - building a milestone or fix batch, reviewing a diff, running a study, or sweeping screens - and a model, a brief, or a review step has to be chosen before dispatch.
---

# Subagent model economy

## Overview

The lead session designs, briefs, decides, integrates and verifies; subagents do the code-heavy and
reading-heavy work on the cheapest model that is trusted for it. Nothing a subagent reports is
treated as done until the lead has checked it.

## Quick reference: which model

| Work | Model (always pass `model`) |
|---|---|
| Implement from a complete brief; draft plans or research | `sonnet` |
| Independent read-only review; any code touching money, spend caps, the model-call path or frozen modules | `opus` |
| Mechanical copying or formatting | `haiku` |
| The top-tier model (e.g. Fable) | never as a subagent unless the user says so |

## The brief is a file with eight parts

Write it to a scratch file; the subagent gets only its path.

1. Repo, branch to create, "status clean first".
2. Hard rules, verbatim: kill processes by PID only; no stash, checkout of files, hard reset or
   worktrees; never read secret files; no deploy; no full test suite; no server left running.
3. Why, in the user's words, with the measured numbers.
4. What to build: names, signatures, constant values, what must not change.
5. The invariant most likely to break, and its test, written first and watched failing.
6. Mutation checks to run.
7. Exact test, lint and build commands.
8. Finish report: files, counts, mutations, and what in the brief was wrong.

## The loop

1. Builder runs in the background.
2. Full test run and reviewer in parallel. The reviewer gets the diff, the brief and the defect
   classes below, never your conclusion.
3. Findings go back to the same builder, only after the test run has released the checkout.
4. Merge, tag, push, deploy with before/after reads.
5. Look at the running result yourself; for UI, screenshots at the user's real screen sizes.

## Defect classes every reviewer is told to hunt

- A cap or spend record that fails open: written only after the paid call; an unreadable record
  read as "nothing spent"; an error reclassified as unbilled.
- A second view (phone, card) using its own words, colours or arithmetic instead of the original's.
- A figure shown where the server declines to give one.
- Features at fit time that differ from decision time (stale prices).
- A "blind" model reaching the answer through a tool.
- A capability shipped "off" that still changes behaviour.

## When one agent is not enough (multi-agent workflow)

- A claimed edge: independent skeptics, one robustness test each, then a judge.
- Competing methods: two analysts, a skeptic, a judge.
- Many screens: one reviewer per group with its own browser, then a ranker who opens every image.

## Common mistakes

| Mistake | Fix |
|---|---|
| Reporting a finding before the skeptic pass | Run the skeptics first |
| Two agents in one checkout | One at a time; a hand-back is not proof it stopped |
| Resuming a builder during the test run | Wait for the result |
| Trusting a subagent's "tests pass" | Rerun the key test; read the diff |
| A review brief stating your verdict | Give code and defect classes only |
