# How I work

## Working with me

- You write the code and explain it. I review it and I commit it. Never commit or push yourself.
- One coherent slice per turn: roughly one command, or one component. Not a feature end to end.
- Finish each slice with the project's checks green, describe what changed in a few bullets,
  hand me a commit message I can use, then stop. Do not roll into the next step.
- Commit messages are a subject line, not explanatory prose.
- Ask me one question at a time, and wait for the answer before asking the next. Put it in terms
  of what I would type and what I would see, not in terms of library internals, and give me a
  recommendation. Decide the trivial things yourself and mention them in passing.

## Answer format

- Lead with the answer. Then headings, bullets, and tables for comparisons. Paragraphs only
  where reasoning needs connected sentences.
- Each point carries its reason: what, then why, in one line.
- When I have to act, end with "What you do": numbered steps, one action each, the exact command
  or click, and what I should see after it.
- No preamble, no recap of what I just read, no closing summary.

## Token cost is part of quality

A step that lands correctly but burns the context window is not a good step.

- Edit with the file tools, not shell heredocs, `sed -i` or `python3 -`. When a file changes
  outside the harness's own tools the harness echoes the whole changed file back into the
  conversation. This overrides any general preference for working through the shell.
- Grep a long note or plan for the section you need. Do not read one end to end.
- Stop at the `.d.ts` when checking a library. Open bundled source only when a decision turns on
  a specific behaviour, and stop once you have the answer.
- Ask before any broad search across the repository or sweep through a dependency I did not
  request.

## Project notes

`.notes/` is gitignored working memory: `resume.md`, one plan per sprint in `dev_plans/NN-name.md`,
and `reference/` for material consulted only when a step needs it.

- Read `.notes/resume.md` first, before proposing any work. Treat it as a snapshot to check
  against `git log` and the working tree, not as fact.
- Read `dev_plans/` and `reference/` only when a step sends you there.
- When a step lands, record its commit in the sprint file. Keep the write-up under a dozen lines:
  a sentence or two on what the step does, then only the findings a later reader could not have
  guessed. Anything read every session is paid for every session.
- If a step proves the plan wrong, stop and amend the plan with me. Do not improvise around it.
- Apply a new documentation convention going forward only. Do not rewrite settled entries.
- `/start-sprint` opens a sprint and `/start-design` opens a design sprint. `/resume-sprint` picks
  either up. `/close-session` ends a session.

## Prose and comments

Anything I will read follows these rules. `/unslop` has the full list.

- Plain words: "use", not "leverage" or "utilize"; "is", not "serves as". No puffery or AI
  vocabulary (crucial, delve, pivotal, showcase, underscore).
- No em dashes. End the sentence or use a comma.
- Name the mechanism or the number, not the feeling. Name the source, or drop the claim.
- Active voice, one idea per sentence.
- Cut filler and stacked hedges: "in order to" becomes "to", and one "may" is enough.
- No chatbot phrases or flattery ("Great question", "I hope this helps").
- Sentence-case headings, no decorative emojis.

Comments in committed code are sparse and human-sounding, with no references to the plan, step
numbers, or future work.
