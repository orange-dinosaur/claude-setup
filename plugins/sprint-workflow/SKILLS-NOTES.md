# Personal skills

Skills here are available in every project. Two were copied from other people's repositories and
then modified locally. **Re-downloading them from upstream will overwrite those changes**, so if
you update one, re-apply the edits listed below.

| Skill | Upstream | Local edits |
|---|---|---|
| `grill-with-docs` | [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/grill-with-docs) (MIT) | model-invocable, so `start-sprint` can call it; the description tells the model not to reach for it unsent |
| `grilling` | [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/productivity/grilling) (MIT) | asks one question per turn instead of a whole numbered round; looks facts up directly instead of dispatching a sub-agent |
| `domain-modeling` | [mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/domain-modeling) (MIT) | writes `.notes/reference/glossary.md` and `.notes/reference/decisions.md` instead of a root `CONTEXT.md` and `docs/adr/`; `CONTEXT-FORMAT.md` renamed to `GLOSSARY-FORMAT.md`; multi-context `CONTEXT-MAP.md` section dropped |
| `unslop` | [cursor/plugins](https://github.com/cursor/plugins/tree/main/pstack/skills/unslop) | none |
| `start-sprint` | local | — |
| `start-design` | local | — |
| `close-session` | local | — |
| `resume-sprint` | local | — |
| `graphify` | [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify), written by its CLI | `disable-model-invocation: true` added to the frontmatter; reinstalling drops it |

`graphify` comes from the `graphifyy` Python tool (`uv tool install graphifyy`). Reinstall or
update the skill with `graphify install --platform claude`, run outside `~/.claude`. Both that
command and `graphify claude install` also append a graphify section to the `CLAUDE.md` in the
current folder, which from `~/.claude` is the global one, so remove it afterwards. `graphify claude
install` is per project: it adds a PreToolUse hook to `./.claude/settings.json`, so run it only
inside a repo that should always consult its graph. Git hooks (`graphify hook install`) are not
installed. Remove everything with `graphify uninstall` and `uv tool uninstall graphifyy`.

`grill-with-docs` does nothing on its own: its whole body is an instruction to invoke `grilling`
and `domain-modeling`. All three must stay installed together.

The upstream repository is a Claude Code plugin marketplace of 25 skills. It is deliberately not
installed as a plugin: that would load 25 skill descriptions into every session's context, and
only these three are wanted.

`unslop`, `start-sprint`, `start-design`, `resume-sprint`, `close-session` and `graphify` set
`disable-model-invocation: true`, so they run only when I invoke them by name.

`grill-with-docs` deliberately does not. `start-sprint` step 3 and `start-design` step 4 hand off to it, and a skill with
that flag set cannot be reached through the Skill tool at all, only by typing `/grill-with-docs`
myself. The cost of leaving the flag off is one more description in every session's context and
the chance the model picks the skill up unprompted, which the last sentence of the description is
there to discourage.

## Plugins

These are installed through `/plugin`, not copied here. `/plugin` lists, disables and removes them.

| Plugin | Marketplace | What it adds | Why |
|---|---|---|---|
| `andrej-karpathy-skills` | `multica-ai/andrej-karpathy-skills` | one model-invoked skill, `karpathy-guidelines`: think before coding, simplicity, surgical edits, verifiable goals | one skill description, no hooks. Where its "loop until verified" pulls against "one slice, then stop", `CLAUDE.md` wins |
| `ponytail` | `DietrichGebert/ponytail` | hooks on SessionStart, SubagentStart and UserPromptSubmit that inject "write less code" rules, plus six skills (`ponytail`, `-review`, `-audit`, `-debt`, `-gain`, `-help`), so six more descriptions per session | its upstream default is `full`; `~/.config/ponytail/config.json` sets `lite` to keep the always-on rules light. `/ponytail full` or `/ponytail off` changes the level for a session. Its statusline badge is not wired in, so the custom statusline stays |
| `frontend-design` | `claude-plugins-official` | one model-invoked skill: guidance on aesthetic direction, typography and avoiding templated defaults | Anthropic's own, no hooks. `start-design` uses it at steps that build pages |

`caveman` (JuliusBrussee/caveman) was considered and not installed. It shortens prose by dropping
grammar and explanation, which conflicts with the answer format in `CLAUDE.md` and with `unslop`.

## Design skills

These install with `npx`, not `/plugin`. `/plugin` does not list them.

| Skill | Upstream | What it adds | Why |
|---|---|---|---|
| `impeccable` | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) (Apache 2.0) | one skill with 24 commands, including `audit` and `polish`. Downloads a checksum-verified engine binary on first use | the review step of a design sprint. Also `/impeccable document`, which generates a `DESIGN.md` from existing code |
| `web-design-guidelines` | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) | reviews named files, fetching the rules from GitHub on each run | a rules check, no cost until it runs |
| `emil-design-eng`, `apple-design`, `mobile-native` | [emilkowalski/skills](https://github.com/emilkowalski/skills) (MIT) | motion and UI polish, Apple-style web interaction, web apps on phones | three of the repo's 16 skills. Web only, so native SwiftUI or Compose apps have no dedicated rules. They reply with one line and stop if invoked without a question |

`impeccable` is deliberately not installed as a plugin. The plugin always adds hooks on SessionStart,
every Edit and Write, and Stop, in every project, and its docs do not say the off switch covers
plugin installs. `--no-hooks` applies to one run, so updates need it too. Its README gives no
uninstall command.

Together with `frontend-design`, these six add about 600 tokens of descriptions to every session.
Their bodies total about 22,000 tokens, which is why `start-design` keeps them out until its review
step.

Considered and not installed: `taste-skill` overlaps with `frontend-design`. `shadcn`, Refero,
`ui-ux-pro-max`, `taste-skill-classics` and `mobile-design-skills` were left out to keep the install
list short.
