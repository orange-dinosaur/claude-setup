---
name: start-audit
description: Open an audit sprint. Maps the codebase cheaply, audits the parts most likely to hold problems with code-review and ponytail-review, triages the findings with me, then writes a plan to fix them.
disable-model-invocation: true
argument-hint: "[path ...]"
---

# Start an audit

The objective is fixed: find bugs, likely problems and things worth improving, then plan the fixes.
Work through these in order. Stop at the end for review; do not begin fixing.

**Never read the whole codebase.** Every file read in this session is resent with every later
request. Map first, read only what I approve, and keep file contents out of the main context.

## 1. Scaffold and profile

If `.notes` or `.notes/resume.md` is missing, do `start-sprint` steps 1 and 2 first. Read that
skill's file for the exact steps; do not improvise them.

## 2. Settle the scope

- **Paths given** (`/start-audit src/api`): those paths are the scope. Skip to step 4 with every
  file under them as a target, unless that is more than about 15 files, in which case do step 3
  inside the paths.
- **No arguments**: the scope is the whole repository, narrowed in step 3.

## 3. Map, then propose targets

Gather this without opening source files:

- `git ls-files` with line counts, largest first.
- The 20 most-changed files: `git log --format= --name-only | sort | uniq -c | sort -rn | head -20`.
- The manifest and the CI workflow, for entry points and check commands.
- Which source folders have tests next to them.

From that, propose about 10 to 15 target files, grouped into areas of related files. Favour files
that are both large and frequently changed, entry points, code that handles outside input, and
code with no tests. Give each a one-line reason. Wait for my OK or edits before reading anything.

## 4. Audit each area

Create `.notes/reference/audit-NN.md`, numbered like the plans, before the first area. For each
area, run both passes and append their findings under a heading for that area:

1. **Bugs.** Call the Skill tool for `code-review` at level `medium` with the area's paths as the
   target. It runs its own reviewers, so call it from the main session.
2. **Over-engineering and other problems.** Start one `general-purpose` subagent for the area. Tell
   it to call the Skill tool for `ponytail:ponytail-review`, treating the area's files as the diff,
   then list any other problems it saw: missing tests on branching logic, swallowed errors, stale
   comments or docs. It returns findings only, one line each, `file:line` first, never file
   contents.

If either skill is not installed, say so once and apply that lens yourself inside the subagent.

Each finding gets a severity: `bug`, `risk` or `cleanup`. Drop duplicates across the two passes.

## 5. Triage

Show me the findings file's path and a count per severity. Then go through the `bug` and `risk`
findings with me one at a time, with your recommendation: fix in this sprint, defer, or drop.
Offer the `cleanup` findings as one batch I can accept, trim or skip. Mark each decision in the
findings file as it is made.

## 6. Write the plan

Write `.notes/dev_plans/NN-audit-<scope>.md` to the contract and sections in `start-sprint` step 4.
Read that section rather than rewriting it from memory. On top of that contract:

- Order steps `bug` first, then `risk`, then `cleanup`.
- Each step names the finding it fixes and links to the findings file.
- A bug fix step includes the check that would have caught it.
- Deferred findings go in a **Deferred** section at the bottom, one line each.

## 7. Point at it

Update `.notes/resume.md` so the live plan is the one just written and the next step is step 1.
Then stop and tell me the plan is ready to read.
