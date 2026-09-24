---
name: resume-sprint
description: Pick up a development sprint from a cold start. Reads .notes/resume.md, checks it against the repository, and proposes the next step.
disable-model-invocation: true
model: sonnet
---

# Resume a sprint

Work through these in order. Stop at the end for my go-ahead; do not begin implementing.

## 1. Read the handoff

Read `.notes/resume.md` end to end. It is short by design and it is the only file here you read in
full.

If it is missing, stop and say so: either this project has no sprint yet and wants `/start-sprint`,
or the notes directory was never created.

## 2. Check it against the repository

`resume.md` is yesterday's snapshot, not fact. Before you trust it, run `git log` for what actually
landed and `git status` for anything still uncommitted, and compare.

Where they disagree, the repository wins. Say which line of the handoff was stale rather than
silently working from the corrected version. If the tree is dirty, work out what is in it before
proposing anything that would touch the same files.

## 3. Find the next step

The handoff names the live plan and the next step by number. Grep
`.notes/dev_plans/NN-name.md` for that step and read only that section, plus the working rules and
the "assumed, not verified" list if the file has them. Do not read the plan end to end, and do not
open older plans or anything in `.notes/reference/` unless the step sends you there.

If the step's write-up says it already landed, or its commit is recorded, move to the one after it
and tell me the handoff was behind.

## 4. Report and stop

Tell me, as a short bulleted report:

- Where the sprint stands: last step committed, working tree clean or what is in it.
- Anything in the handoff that no longer matches the repository.
- The next step by number and title, what it touches, and what "done" means for it.
- Any open fork the step is tagged to ask. Ask it now, one question at a time, with a
  recommendation, since the answer shapes the step.

Then stop and wait. The next turn implements that one step and nothing beyond it.
