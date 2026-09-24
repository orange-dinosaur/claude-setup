---
name: start-sprint
description: Open a development sprint. Scaffolds .notes/ if missing, grills the sprint until the design is settled, then writes a plan document a cold agent can execute.
disable-model-invocation: true
---

# Start a sprint

Work through these in order. Stop at the end for review; do not begin implementing.

## 1. Scaffold, if this project has no notes yet

If `.notes/` is missing, create `.notes/`, `.notes/dev_plans/` and `.notes/reference/`, and append
to `.gitignore`:

```
# Working notes, not part of the project
.notes/
```

If the repository has no `.gitignore`, create one with just that entry.

## 2. The project profile, if `resume.md` is missing

`.notes/resume.md` opens with a profile a cold agent needs before it can do anything. Fill in what
you can read from the repository yourself: the package manager comes from the lockfile, the check
commands from the scripts in the manifest and the CI workflow. Ask me only what the repository
cannot tell you, one question at a time.

The profile records: what this project is in two sentences, the package manager, the exact
commands that must go green before a step is done, and how I verify the result myself.

## 3. Grill the sprint

Call the Skill tool for `grill-with-docs`. Do not write the plan until the design tree's frontier is empty and I
have confirmed we have a shared understanding. Terms and hard-to-reverse decisions get written to
`.notes/reference/` as they settle, which is that skill's job.

## 4. Write the plan

Scan `.notes/dev_plans/` for the highest existing number and write
`.notes/dev_plans/NN-kebab-name.md`, two digits, numbering from 01.

**The contract: a fresh agent with no access to this conversation must be able to execute the plan
without problems.** Everything it needs is in the document or reachable from it. Write it to that
standard from the start rather than a terse table of steps.

Sections:

- **Working rules for this repo.** The exact commands that must go green, that I commit, one step
  per turn, that design forks are asked one at a time. Anything project-specific that overrides
  the global rules.
- **Orientation.** The parts of the existing code this sprint touches, and the gotchas already
  paid for. Enough to start, not a tour.
- **Decisions this rests on.** Restated inline, not only linked. A cold agent should not have to
  open `reference/decisions.md` to understand the plan.
- **Assumed, not verified.** An explicit list of what was taken on faith, with an instruction to
  correct this document as each one is established.
- **Steps**, numbered from 1. Each step is small, self-contained and independently committable.
  Each names the files it touches and what "done" means. Leave room for a commit hash and a short
  write-up to be appended when it lands.
- **Open forks**, at the bottom, each tagged with the step that will ask it. Do not ask them now.

A step that spans both halves of the stack is too big. Split it.

## 5. Point at it

Update `.notes/resume.md` so the live plan is the one just written and the next step is step 1.
Then stop and tell me the plan is ready to read.
