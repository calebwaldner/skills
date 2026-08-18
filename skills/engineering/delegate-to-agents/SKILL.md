---
name: delegate-to-agents
description: Staff delegated work with specialists from The Agency catalog — pick, install missing agents after a yes, then hand execution to the delegate skill.
argument-hint: "The work to delegate"
disable-model-invocation: true
---

# Delegate to Agents

The staffing layer in front of the `delegate` skill: `delegate` executes work through subagents; this skill decides *who* those subagents are. The candidate pool is The Agency ([msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents)) — a growing catalog of 270+ specialist agents, one markdown file each, across engineering, design, security, marketing, and a dozen more divisions. Installed specialists live at `~/.claude/agents/`; everything else is one fetch away. The catalog's roster index is 1,100+ lines and always current on GitHub, which sets the shape of everything below: the roster is read live, never cached here — and never loaded into this session, because a lean session context is the thing this whole delegation flow exists to protect.

## Steps

1. **Scout.** Dispatch a subagent carrying: a summary of the work, the roster URL `https://raw.githubusercontent.com/msitarzewski/agency-agents/main/README.md`, and this return format — for each distinct kind of work in the request: agent `name` (Title Case, as in the file's frontmatter), division, filename, one-line fit; or `no fit` plus a word on why. Instruct it to staff with the fewest agents that cover the work. Done when every distinct kind of work has a named specialist or an explicit no-fit.

2. **Mark installed.** List `~/.claude/agents/` and mark each pick installed or missing. Catalog filenames are division-prefixed (`engineering-frontend-developer.md`), so a filename match settles it. Done when every pick carries one of the two marks.

3. **Ask, then install.** Show the user the staffing plan — each pick's name, division, and one-line fit, with the missing ones flagged for install — and ask one yes/no for the missing set. On yes: fetch each `https://raw.githubusercontent.com/msitarzewski/agency-agents/main/<division>/<filename>` and write it to `~/.claude/agents/<filename>` unchanged. Claude Code watches that directory, so new agents register within seconds — no restart (the one exception: if `~/.claude/agents/` itself didn't exist when the session started, the watcher never bound, and a restart is the honest instruction). Done when every approved file is on disk; a declined pick is dropped and its task staffs generic in step 4.

4. **Hand off.** Invoke the `delegate` skill with the work, staffing each task's agent by its frontmatter `name` — the Agent tool dispatches by `name`, not filename. Tasks with no specialist run on generic subagents. Done when delegation is underway under `delegate`'s own steps and the final report names any staffing gap — a no-fit or a declined install — in one line.

## Rules

- **The roster never enters this session.** The scout reads the 1,100-line index and returns a decision-sized answer. Fetching it here would spend the very context this flow exists to keep lean, and caching a copy in this skill would rot against an active catalog. Live fetch, remote eyes.

- **A yes gates every install; hands do nothing.** These are third-party prompts joining the user's standing roster, so nothing lands in `~/.claude/agents/` without their yes — but after the yes the copying is yours, not theirs. Show the pick, get the yes, write the file.

- **Staffing never dead-ends the work.** No specialist fits, or the user declines an install: hand off to `delegate` with generic subagents and name the gap in the report. The work was the point; the catalog is a means.

- **Install verbatim, under the catalog filename.** The Title Case `name` inside the file is what the Agent tool dispatches by; the division-prefixed filename is what step 2 matches against next time. Editing either breaks one of the two lookups.

- **Always list who is assigned to each task.** Once you have decided on the staffing, list them right away so the user can see the assignments clearly; this is done immediately after the staffing decisions are made.