# skills

Caleb's personal agent skills library — reusable instruction sets for Claude Code and other agents, spanning engineering, church work, productivity, and whatever else earns a skill. Anything scoped to a specific project lives in that project's repo instead.

## Install

Uses the [`skills`](https://www.npmjs.com/package/skills) CLI, which installs `SKILL.md` files into whichever agents you target.

```bash
# Install from this repo (interactive — pick skills and agents)
npx skills add calebwaldner/skills

# See what's in here without installing
npx skills add calebwaldner/skills --list

# Install one skill, globally, to Claude Code
npx skills add calebwaldner/skills --skill design-handoff -g -a claude-code

# Install everything, everywhere, no prompts
npx skills add calebwaldner/skills --all
```

Scope is per-project by default (`.claude/skills/`); `-g` installs to `~/.claude/skills/` so the skill is available across all projects. Symlinking is the default and recommended install method — it keeps a single canonical copy, so `npx skills update` picks up changes here.

```bash
npx skills update              # update installed skills to latest
npx skills list                # show what's installed
npx skills remove design-handoff
```

## Skills

| Skill | Category | What it does |
| --- | --- | --- |
| `design-handoff` | engineering | Compacts a grilled feature thread into a self-contained Claude Design handoff doc with a two-round prototype ask. |
| `delegate` | engineering | Delegate coding work to subagents. |
| `delegate-to-agents` | engineering | Staffs delegated work with specialists from The Agency catalog, installing missing agents after a yes, then hands execution to `delegate`. |

## Layout

```
skills/<category>/<skill-name>/SKILL.md
```

One directory per skill. The category directory is organizational only — the CLI discovers skills at any depth, so grouping is free.

## Adding a skill

```bash
npx skills init skills/<category>/<skill-name>
```

Then fill in the `SKILL.md`. See `CLAUDE.md` for the conventions this library follows — frontmatter, the Steps/Rules/Template shape, and why the rules are written to explain themselves.

Ideas that aren't skills yet live in [`IDEAS.md`](IDEAS.md) — a someday/maybe list with notes and open questions per idea.
