---
name: start-sprint
description: Open a development sprint. Scaffolds .notes as a link into development_notes if missing, grills the sprint until the design is settled, then writes a plan document a cold agent can execute.
disable-model-invocation: true
---

# Start a sprint

Work through these in order. Stop at the end for review; do not begin implementing.

## 1. Scaffold, if this project has no notes yet

Notes live in the shared `development_notes` repository next to the project, one folder per
project, and the project's `.notes` is a symlink into it. If `.notes` already exists, as a link or
a real folder, leave it alone.

Otherwise, from the project root, with `<project>` as the root folder's name:

1. If `../development_notes` does not exist, stop and ask me where it is.
2. Create `../development_notes/<project>/.notes/dev_plans/` and
   `../development_notes/<project>/.notes/reference/`.
3. Link it: `ln -s ../development_notes/<project>/.notes .notes`.

Then append to `.gitignore`, unless it already ignores `.notes`:

```
# Working notes, not part of the project
.notes
```

No trailing slash: `.notes/` matches only a directory, so git would track the symlink. If the
repository has no `.gitignore`, create one with just that entry.

Committing the notes is my job, in `development_notes`.

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

**The level of detail: the executing agent types, it does not design.** Every name and every
behaviour is decided in the plan. If a step leaves the agent to pick a function name, a column
type, an error message or what happens at an edge, the step is not finished. Expect 30 to 50 lines
per step and 200 to 350 for the plan. Do not trim it to look tidy.

Before writing, read the code the sprint touches, so the plan quotes real paths, signatures and
props instead of guessing them. What you could not check goes under "Assumed, not verified".

### Sections, in this order

- **Title and goal.** `# Sprint N: Name`, then a `**Goal.**` paragraph: what a user can do when
  the sprint is over, in two or three sentences.
- **Working rules for this repo.** Five bullets at most: the exact commands that must go green,
  written out in full; that I commit and add packages; one step per turn; design forks asked one
  at a time with a recommendation; stop if the code disagrees with the plan. Add the words to use
  in anything a user reads, and the words never to use, from the glossary.
- **How to read a step.** Copy the block below into every plan, adjusting only the file it names.
- **Orientation.** Bullets on the existing code this sprint touches: the real function signatures,
  component props, table columns and patterns the steps will call or copy, each with its path. Add
  the gotchas already paid for. Enough to start, not a tour.
- **Decisions this rests on.** Each one a bold statement and its reason, restated inline, not only
  linked. A cold agent should not have to open `reference/decisions.md` to understand the plan.
  Say what is deliberately left to a later sprint.
- **Assumed, not verified.** A numbered list of what was taken on faith.
- **Steps**, as described below.
- **Open forks**, at the bottom. Each one names the step that will ask it, the options, and a
  recommendation. Do not ask them now.

### The "How to read a step" block

```markdown
## How to read a step

Every step has the same parts, in this order:

1. **What this step does**: the result, and what is deliberately left for later.
2. **How it fits together**: which file calls which.
3. **Order of work**: numbered actions. Do them in that order, one at a time.
4. **Files**: the exact content of each file. Names of files, functions, parameters, columns and
   every sentence shown to the user are given here and are to be used as written.
5. **Done**: the checks that must pass before the step is finished.

Before step 1, settle every item under "Assumed, not verified" and write the answer into this
plan. If something you need is not written in the step, in `reference/conventions.md`, or in the
code already there, **stop and ask. Do not invent a name, a behaviour or a sentence.** A sentence
tagged **DRAFT** was not yet read by me: show it and wait before using it.
```

### Steps

`### Step N: short name`, numbered from 1. Each step is small, self-contained and independently
committable. A step that spans both halves of the stack is too big. Split it. Each step has the
five parts, as bold run-in labels:

- **What this step does.** Two or three sentences: the result a user or caller gets, and what is
  left for a named later step.
- **How it fits together.** A paragraph on the mechanism: which file calls which, what each table
  or function is for, and the one design point that makes it work, such as the constraint that
  enforces a rule or the single helper every page goes through.
- **Order of work.** Numbered actions, one per line, in the order to do them, ending with the
  check command. Include the generate and migrate commands where a schema changes.
- **Files.** One bullet per file, with its full path, and sub-bullets for what goes in it:
  - a table: every column with its type, nullability, default, maximum length, what happens on
    delete, and the keys;
  - a function: its full signature, who may call it, what it returns, and each refusal with the
    exact error text;
  - a page or component: its props, who may open it and what the others get, its sections in
    order, and every label, button, hint, empty state and message, word for word;
  - an email: subject and body, word for word;
  - tests: one clause per test, naming the behaviour it pins. Prefer the refusals and the cases
    where something must stay hidden.

  Where a step changes a file an earlier step wrote, say what is added. Where it copies an
  existing pattern, name the function or sprint it copies from.
- **Done.** The check command, plus what I do in the browser or terminal and what I see, when the
  checks cannot prove it.

Tag with **DRAFT** every user-facing sentence I did not approve during the grill. Sentences I did
approve, or that already exist in the code, carry no tag.

The last step also updates `reference/conventions.md` with the patterns this sprint added, and
`resume.md` to say the sprint is done.

Leave each step room for a commit hash and a short write-up to be appended when it lands.

### Before you stop

Read the plan once as the cold agent. For each step ask: is there a name, type, sentence or edge
case I would have to make up? Either write it in, or list it under "Assumed, not verified" or
"Open forks".

## 5. Point at it

Update `.notes/resume.md` so the live plan is the one just written and the next step is step 1.
Then stop and tell me the plan is ready to read.
