---
name: design-handoff
description: Compact a grilled feature thread into a Claude Design handoff doc — a self-contained functional spec with a two-round ask.
argument-hint: "Feature slug, plus anything unusual about the ask (e.g. one round only, mobile-first)"
disable-model-invocation: true
---

# Design Handoff

The bridge from a grilled feature thread to **Claude Design** — an external prototyping tool that produces team-shareable interactive HTML and already holds the app's **design system** in its project knowledge. Claude Design *can* link a repo, but this doc deliberately doesn't lean on that: the repo records what the code does today, not what you decided, and pointing a redesign at today's implementation biases it toward parity. So the doc is a self-contained **functional spec**: behavior, states, and data live in the doc; design knowledge travels by naming the loaded system (Stratus: the `tu` system), never by re-educating on it.

## Steps

1. **Gate on settled decisions.** Sweep the thread and ticket for questions that would change layout, states, or visible data. The doc records decisions, not open questions — if any remain, list them and resume grilling instead of writing. Done when every template section can be filled from a decision made in the thread.
2. **Write the doc** at `docs/handoffs/YYYY-MM-DD-<feature-slug>.md`, gitignored — add `docs/handoffs/` to the repo's `.gitignore` if it isn't there yet. Done when every numbered section is filled, or replaced with a one-line reason it doesn't apply (e.g. "no mobile presentation — desktop-only admin tool").
3. **Print the kickoff** for the user:
   - the attachment checklist from §10,
   - an opening prompt that invites interrogation before any building (e.g. "Attached is a handoff doc for <feature>. Read it and ask me anything that would change layout, states, or data. Then run round 1 per §2."),
   - the return path: bring the round-2 prototype decisions back to the originating grilling thread, then `/to-spec` there.

   Claude Design's questions are a free audit of the doc. Anything it asks that §1–§9 should have answered is a gap — answer it in the thread and patch the doc before round 2. Invite questions; don't delegate decisions: the ask in §2 is yours to make.

## Rules

- **Throwaway.** The doc is a compiler input — compiled from the thread, consumed by Claude Design, spent. It is never committed, and nothing downstream reads it: `/to-spec` synthesizes the originating thread, not this file. It stays on disk only so you can re-attach it to a fresh Claude Design session. (`/prototype` sets the convention this follows: main keeps only the validated decision.)
- **Self-contained.** Claude Design can link a repo — don't rely on it. The repo holds no decisions: anchors, freedoms, and which quirks are fair game to fix are not inferable from code, and a linked repo pulls a redesign toward reproducing today's implementation. Every behavior, field, and state the prototype must honor is written out in the doc. How deeply a linked repo is read per session is undocumented, so a spec's completeness is not something to bet on it.
- **Design system by name, not by education.** One clause in §1 names the loaded system. No token tables, no attached CSS, no style guidance — Claude Design inherits the design system per project and checks its own output against it before you see it, so an attached `design-system.css` is redundant at best. If the loaded system is stale or wrong, fix it at the source with `/design-sync` from Claude Code rather than patching it per-session with attachments.
- **Current-UI screenshots are functional reference, not the visual target.** Say so wherever they're listed in §10.
- **Two-round ask** is the default shape: lightweight direction exploration first, then one full interactive build of the winner. Deviate only when the user's arguments say so.
- **Anchors vs. freedoms.** Sort every constraint into anchor (non-negotiable) or freedom (explicitly released) — ambiguity between the two is what makes prototypes miss.
- **Realistic domain fake data.** Name the flavor in §2 (for Contractors Cloud: project names like "Smith Residence – Roof Replacement", street addresses, insurance companies, claim numbers, statuses) so the team preview feels real.
- **Fake data only.** Redact secrets and real customer PII from anything mined out of the thread or ticket.

## Template

```markdown
# Handoff: <Feature> — Claude Design Prototype

**Ticket:** <GitHub issue link> / <CCDEV-####>
**Date:** <YYYY-MM-DD>
**Author:** Caleb Waldner (grilled via Claude Code; decisions recorded below)
**Audience:** Claude Design session producing a shareable HTML prototype

---

## 1. Context

<One paragraph: the app and domain in a sentence; what this feature is and
where it lives; the goal framing — design-system redesign with exact
functional parity, new feature, or behavior change. End by naming the design
system: "Design in the `tu` design system already loaded in your project
knowledge.">

## 2. The ask (two rounds)

**Round 1 — direction exploration.** Produce **2–3 distinct layout
directions**. For each, <the 2–3 states that reveal the layout> are enough.
We will pick one direction.

**Round 2 — full interactive build.** One HTML prototype of the chosen
direction, shareable with the team for hands-on testing. Must cover: all
states (§4), edge/empty states (§6), working interactivity (<list: keyboard
nav, live filtering against fake data, …>), and the mobile presentation (§7).

Use realistic <domain> fake data (<examples>) so the team preview feels real.

Ask before you build: anything here that would change layout, states, or data
is worth a question first.

## 3. Design anchors vs. freedoms

**Anchors (non-negotiable):**

- <behaviors and affordances that must survive any layout>

**Freedoms:**

- <what may change: overall anatomy, row shapes, the quirks in §8, …>

## 4. Functional spec — states and behavior

<One subsection per state: open/close, idle, active, loading, view-all, ….
Exact triggers, timings (debounce ms), ordering rules, counts and caps, reset
semantics. Include a selection-semantics table: each selectable item → real
behavior → what the prototype should simulate (stub toast/dialog vs. real
in-prototype navigation).>

## 5. Data displayed

<Per entity or section, a field table: field, notes (highlighted? fallback?
today's responsive hiding). State which fields must remain visible somewhere
— row, expansion, or preview.>

## 6. Edge/empty states to cover (round 2)

- <zero-data variants, loading states, missing-field fallbacks>

## 7. Mobile

<Today's mobile presentation in brief, then the constraints that survive
redesign: same functions, same data, same selection semantics, touch-first,
no hover-dependent affordances.>

## 8. Known quirks — fair game to fix

These exist today and are explicitly **not** parity requirements:

1. <quirk>

Final calls happen at prototype review.

## 9. Out of scope

- <adjacent things not to design: already-redesigned neighbors, hidden/beta
  affordances, backend behavior treated as fixed input>

## 10. Assets attached to the Claude Design session

Feature-specific assets only — the design system is already loaded; attach
nothing for it.

- [ ] <N> screenshots of the **current** UI (<which states>) — functional
      reference only, not the visual target
- [ ] <ticket assets, neighbor-UI context screenshots>

## 11. After the prototype

(Notes for you, not Claude Design.) Prototype review → decisions back to the
originating grilling thread → `/to-spec` there → implementation in <target
codebase / components> on branch `<branch>`.
```
