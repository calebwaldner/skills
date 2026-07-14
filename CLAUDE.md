# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Caleb's personal skills library — Claude Code skills spanning every area of interest, not just engineering: code, church work, productivity, recreation. Anything that doesn't belong to a specific project repo lives here.

This is a content repo, not an application. There is no build, no lint, and no test suite. `pnpm test` is still the `npm init` placeholder that exits 1. `package.json` exists mainly to pin the package manager (pnpm 10.15.1) and name the repo — this repo is **not** an npm package and isn't published.

## Consumption

Skills are installed via the [`skills`](https://github.com/vercel-labs/skills) CLI (`npx skills add calebwaldner/skills`), which reads `SKILL.md` files straight from the GitHub repo and symlinks them into `~/.claude/skills/` or a project's `.claude/skills/`. Practical consequences:

- **`main` is the distribution channel.** There's no publish step between a commit here and what `npx skills update` pulls down. A half-finished skill on `main` is a half-finished skill on Caleb's machine.
- **The one real verification step** is `npx skills add ./ --list` from the repo root — it runs the CLI's actual discovery against the working tree and prints the skills it found with their descriptions. If a new skill doesn't appear there, its frontmatter is malformed. Use this instead of eyeballing the YAML.

## Layout

```
skills/<category>/<skill-name>/SKILL.md
```

One directory per skill, one `SKILL.md` inside it. The category directory (`engineering/`, and whatever domains come later) is the only grouping mechanism — expect it to widen well past engineering.

Nesting is safe: the CLI discovers skills at any depth and keys them by the frontmatter `name`, not the path (verified — `skills/engineering/design-handoff/` resolves as `design-handoff`). So categories are free to add, but skill names must stay globally unique across the repo, and renaming a directory doesn't rename the skill.

## Skill authoring conventions

These come from `skills/engineering/design-handoff/SKILL.md`, the first skill written here. It's the reference for shape; follow it unless there's a reason not to.

**Frontmatter.** `name` and `description` always. `description` is what the model reads to decide relevance, so it states the transformation the skill performs, not the topic it's about. Add `argument-hint` when the skill takes arguments, and `disable-model-invocation: true` for skills that should only fire when Caleb explicitly types `/<name>` — `design-handoff` sets this because it writes and commits a file.

**Body.** Open with a paragraph establishing the skill's *situation* — what tool or audience it serves, what that audience can and cannot see — before any instructions. `design-handoff` spends its opening paragraph on the fact that Claude Design has no repo access, because every rule in the skill follows from that one constraint. Then:

- `## Steps` — numbered, each with an explicit done-condition. Not "write the doc" but "done when every numbered section is filled, or replaced with a one-line reason it doesn't apply."
- `## Rules` — bolded lead-in per rule, then the reasoning. These are the constraints that outlive the steps.
- `## Template` — a fenced block the skill fills in, when the output is a document.

Write rules that say *why*, not just what. The value of these skills is the reasoning they encode; a rule stated without its reason gets misapplied at the first edge case.

## Domain context

Skills under `engineering/` are written against Caleb's work at ThinkUp Tech — Contractors Cloud (Stratus, the PHP/Vue frontend; plus the API, mobile, and portal codebases) and Galaxy Forms. See `~/CLAUDE.md` for the full project map and local dev setup. Skills may reference that domain concretely (Stratus's `tu` design system, claim numbers, project names like "Smith Residence – Roof Replacement") — it's a personal library, so specificity beats false generality.

