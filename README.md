# Claude Code setup

My Claude Code configuration: a global `CLAUDE.md` with how I like to work, a plugin with my sprint
workflow skills, and the third-party plugins I use.

## What is in here

| Path | What it is |
|---|---|
| `CLAUDE.md` | Global instructions Claude reads in every project |
| `plugins/sprint-workflow/` | Plugin with 7 skills (table below) |
| `plugins/sprint-workflow/SKILLS-NOTES.md` | Where each skill comes from and why it is set up this way |
| `.claude-plugin/marketplace.json` | Lets Claude Code install this repo as a plugin marketplace |

| Skill | What it does | How it runs |
|---|---|---|
| `start-sprint` | Opens a sprint: creates `.notes/`, questions you about the plan, writes a plan file | You type it |
| `resume-sprint` | Picks a sprint back up from `.notes/resume.md` and proposes the next step | You type it |
| `close-session` | Writes `.notes/resume.md` so the next session can continue | You type it |
| `unslop` | Rewrites text to remove AI-sounding phrasing | You type it |
| `grill-with-docs` | Questions you about a plan and records decisions and terms as it goes | You type it, or `start-sprint` calls it |
| `grilling` | Questions you about a plan, one question at a time | Claude picks it up when relevant |
| `domain-modeling` | Keeps a glossary and decision log in `.notes/reference/` | Claude picks it up when relevant |

`grill-with-docs` only calls `grilling` and `domain-modeling`, so the three have to stay together.
The plugin installs all of them.

## Before you start

You need Claude Code installed and logged in. Check with:

```
claude --version
```

You should see a version number. If not, follow https://docs.claude.com/en/docs/claude-code/setup.

## Install

1. Copy the global instructions. If you already have a `~/.claude/CLAUDE.md`, back it up first,
   because this overwrites it.

   ```
   mkdir -p ~/.claude
   curl -fsSL https://raw.githubusercontent.com/orange-dinosaur/claude-setup/main/CLAUDE.md -o ~/.claude/CLAUDE.md
   ```

2. Start Claude Code in any folder:

   ```
   claude
   ```

3. Inside Claude Code, add this repo as a marketplace:

   ```
   /plugin marketplace add orange-dinosaur/claude-setup
   ```

   You should see a message that the `claude-setup` marketplace was added.

4. Install the skills plugin:

   ```
   /plugin install sprint-workflow@claude-setup
   ```

5. Add and install the third-party plugins:

   ```
   /plugin marketplace add DietrichGebert/ponytail
   /plugin install ponytail@ponytail
   /plugin marketplace add multica-ai/andrej-karpathy-skills
   /plugin install andrej-karpathy-skills@karpathy-skills
   /plugin install frontend-design@claude-plugins-official
   ```

   - `ponytail` pushes Claude towards the simplest code that works. Switch its strength with
     `/ponytail lite`, `/ponytail full` or `/ponytail ultra`.
   - `andrej-karpathy-skills` adds guidelines against common LLM coding mistakes.
   - `frontend-design` is Anthropic's guidance for UI that does not look templated.

6. Quit Claude Code with `/exit`. In your normal terminal, install the design skills. You need Node
   for `npx`.

   ```
   npx impeccable install --providers=claude --scope=global --no-hooks
   ```

   ```
   npx skills add vercel-labs/agent-skills --skill web-design-guidelines -g -a claude-code -y
   ```

   ```
   npx skills add emilkowalski/skills --skill emil-design-eng --skill apple-design --skill mobile-native -g -a claude-code -y
   ```

   - `impeccable` adds `/impeccable audit`, `/impeccable polish` and other review commands. Keep
     `--no-hooks`, or it scans every UI file Claude edits.
   - `web-design-guidelines` reviews named files against Vercel's interface rules.
   - `emil-design-eng`, `apple-design` and `mobile-native` cover motion, Apple-style web interaction
     and web apps on phones.

7. Start Claude Code again with `claude`.

8. Check that everything loaded. Type `/plugin` and open the installed tab. You should see
   `sprint-workflow`, `ponytail`, `andrej-karpathy-skills` and `frontend-design` listed as enabled.
   Then type `/` and you should see `start-sprint`, `resume-sprint`, `close-session`, `unslop` and
   `impeccable` in the list.

## Daily use

1. In a project folder, run `claude` and type `/start-sprint` to plan a new piece of work.
2. Work through the plan one step at a time.
3. Before you stop for the day, type `/close-session`.
4. Next time, type `/resume-sprint` to continue where you left off.

The skills keep their notes in a `.notes/` folder in your project. Add `.notes/` to the project's
`.gitignore` so it stays out of your commits.

## Update

When this repo changes, run inside Claude Code:

```
/plugin marketplace update claude-setup
```

Then restart Claude Code. `CLAUDE.md` does not update by itself: repeat install step 1 to get the
new version.

To update the skills from install step 6, run these in your normal terminal:

```
npx impeccable update --no-hooks
```

```
npx skills update -g
```

## Make it yours

- Edit `~/.claude/CLAUDE.md` freely. It is your copy and updates from this repo only happen when you
  run install step 1 again.
- Turn off a plugin you do not want with `/plugin`, pick it, and choose disable.

## Uninstall

```
/plugin uninstall sprint-workflow@claude-setup
/plugin marketplace remove claude-setup
```

To remove the design skills, run this in your normal terminal:

```
npx skills remove --global web-design-guidelines emil-design-eng apple-design mobile-native
```

Then delete or edit `~/.claude/CLAUDE.md`.

## Credits

`grilling`, `grill-with-docs` and `domain-modeling` are modified copies of skills from
[mattpocock/skills](https://github.com/mattpocock/skills) (MIT). `unslop` comes from
[cursor/plugins](https://github.com/cursor/plugins/tree/main/pstack/skills/unslop).
See `plugins/sprint-workflow/SKILLS-NOTES.md` for the changes.
