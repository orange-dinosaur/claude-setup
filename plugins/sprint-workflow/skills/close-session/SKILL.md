---
name: close-session
description: End a development session. Replaces .notes/resume.md with a fresh handoff a cold agent can pick up from tomorrow.
disable-model-invocation: true
---

# Close the session

Replace the entire contents of `.notes/resume.md`. Do not append to it and do not preserve the old
text: a resume file that accumulates is a resume file nobody reads.

First check where things actually stand. Run `git log` for what landed today and `git status` for
anything still uncommitted, and make sure the sprint plan's step entries match. If a step landed
without its commit recorded, record it now.

## What goes in

Write it as a message from me to an agent starting cold tomorrow morning, in the first person.
Around forty lines. In this order:

1. **What this project is**, and the profile: package manager, the exact commands that must go
   green, how I verify the result myself.
2. **Where to start reading**, and what not to read. Name the live plan file and say to grep it
   for the step rather than reading it end to end. Say explicitly that the older plan documents
   and the reference files are not to be opened unless a step sends you there.
3. **Which step is next**, by number and title, and which steps are already done and committed.
4. **What landed today**, in a few lines, if it is not obvious from the plan.
5. **Open threads**, if any. Flag each one as "mention it if it becomes relevant, but don't act on
   it without asking".

## What stays out

- The working rules. They live in the global `CLAUDE.md` and are already loaded. Repeating them
  here means paying for them twice.
- Anything the sprint plan already says. Point at the plan instead.
- Reasoning that justifies a decision already made. If it matters, it belongs in
  `.notes/reference/decisions.md`, not here.

Uncommitted work is the one thing an agent cannot recover on its own, so if the tree is dirty, say
what is in it and why it was left that way. `.notes/` is gitignored, so there is nothing to commit
here: just tell me the resume file is rewritten.
