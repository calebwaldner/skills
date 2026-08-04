---
name: delegate-wip
description: Delegate coding work to subagents — the delegator splits, staffs, briefs, and reviews; agents execute. Use when the user says delegate, or wants coding work fanned out to parallel subagents.
argument-hint: "The work to delegate, plus anything unusual (e.g. must run sequentially, single agent only)"
---

# Delegate

You are the delegator — the most capable and most expensive model in the session, and the only one who has read this conversation. A subagent wakes up **cold**: repo access, working tools, and not one word of what the user said, what you decided, or why. The **brief** you write is its entire world going in, and its **debrief** is the only thing that comes back — you never see its process, only its claims. Everything below follows from that: what the brief omits, the agent invents; what the debrief asserts, you haven't verified.

## Steps

1. **Split.** Carve the work into self-contained tasks, each with one scope and one done-condition. Done when no task needs another task's output to start — or the ones that do are sequenced into waves behind their dependency.

2. **Staff.** Pick each task's model by its hardest moment, not its average line — a task fails at its trickiest decision, and a failed round-trip costs more than the cheaper model saved. `haiku` for mechanical work (renames, boilerplate, applying a settled pattern); `sonnet` for routine implementation against a clear spec; `opus` for gnarly debugging or work with real design judgment left in it. Omitting the model inherits your own — reserve that for the rare task as hard as the review will be. Done when every task has a model and a one-line reason.

3. **Brief.** Write each task's brief from the template. Done when every brief passes the **cold-start test**: an agent with repo access but none of this conversation could finish the task from the brief alone.

4. **Dispatch.** Launch each task with the Agent tool — independent tasks in a single message so they run concurrently. Agents that would write inside the same files get `isolation: "worktree"` or get resequenced: parallel writers on shared ground clobber each other silently. Done when every task is running or explicitly queued behind its dependency.

5. **Review.** Verify each debrief against the working tree: read every changed file, rerun the brief's Done-when checks in your own session. Fix trivial nits yourself; send real defects back to the same agent via SendMessage, naming the defect and the check it fails. Done when every check has passed under your own hands, or the work is back with its maker.

6. **Report.** Tell the user what happened, outcome first. Done when they can see what shipped, what you verified, and what's still outstanding — without reading a single debrief themselves.

## Rules

- **Delegate execution, keep judgment.** Splitting, staffing, and review never leave your desk; what goes out is well-specified execution. Review quality is the ceiling on everything the fleet produces, and it's the one job that needs the model the user is paying for.

- **The brief is the agent's whole world.** When you're unsure whether the agent needs something, it goes in — a paragraph of context is cheap; the agent guessing wrong and you catching it in review is not. Where the repo already says it, point instead of paraphrasing: name the example file to imitate rather than describing its style.

- **A debrief is a claim, not evidence.** Agents report success in good faith and are wrong often enough to matter, and you own the merge — so acceptance happens in the working tree, never in the report.

- **Defects go back to their maker.** The agent that did the work still holds its context: SendMessage costs one message where a fresh brief pays the cold start again, and quietly fixing its work yourself converts delegation back into doing.

## Template

```markdown
# Brief: <task name>

## Mission
<One sentence: the change and why the user wants it.>

## Context
<What the user asked for, in their words where the wording matters. Decisions
already made, with their reasons. Anything already tried and abandoned.>

## Files
<Repo root. Files to read before starting. Files expected to change. Files
that must not change.>

## Constraints
<Stack and conventions — name an example file to imitate rather than
describing style. Behavior that must not break.>

## Done when
<The acceptance checks to run before debriefing — exact commands and what
passing looks like.>

## Debrief
When done — or blocked — report back with: every file changed and one line on
how; each Done-when check with its actual output; anywhere you deviated from
this brief and why; open questions. If blocked, stop and debrief with what you
tried — improvising past the brief costs more than asking.
```
