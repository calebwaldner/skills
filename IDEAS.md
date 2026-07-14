# Ideas

Someday/maybe list for skills. Nothing here is committed to — an entry earns a
directory under `skills/<category>/` only once it's clear what transformation it
performs and what the hard part is.

Entries are notes, not specs. Capture the itch while it's fresh, capture the open
questions that would otherwise get re-discovered later, and leave it alone. If an
idea can't say what it does *to* something, it's a topic, not a skill — see the
frontmatter conventions in `CLAUDE.md`.

Format per entry: name, one-line pitch, why (the itch it scratches), notes, open
questions. Drop any of those that have nothing to say yet.

---

## `sound-like-me`

**Pitch:** Rewrite Claude-drafted prose in Caleb's voice, calibrated against a
corpus of his actual writing.

**Why:** Claude's default register is recognizable, and anything that goes out
under my name — Slack messages, PR descriptions, ticket comments, docs — reads as
machine-written unless I rewrite it myself. Describing a voice in adjectives
("direct, dry, no hedging") doesn't transfer; examples do.

**Notes:**

- The value is in the corpus, not the instructions. A rule like "avoid hedging"
  is the kind of thing every writing skill already says and no model applies
  consistently. Enough real samples let it pattern-match instead of follow rules.
- Voice is probably not one thing. A Slack reply to a coworker, a PR description,
  and a church email are different registers of the same person. Might need one
  skill with per-context sample sets rather than a single undifferentiated pile.
- Likely wants a rewrite mode (here's a draft, make it sound like me) and a draft
  mode (here's the situation, write it as me). The rewrite mode is the easier one
  to make good and probably where it should start.

**Open questions:**

- Where does the corpus come from? Slack history, PR descriptions, and past
  emails are the obvious sources; all three need mining and cleaning.
- **This repo is public.** Real writing samples mean real content about ThinkUp,
  Contractors Cloud, coworkers, and church going to GitHub. Either the corpus is
  scrubbed hard, or the samples live outside the repo (`~/.claude/` or a private
  repo) and the skill points at them — which breaks the `npx skills add` install
  story. Worth deciding before writing a line of it.
- Do supporting files travel? The install path symlinks `SKILL.md`; whether a
  sibling `references/` directory comes along is untested. If it doesn't, the
  corpus has to be inline in the `SKILL.md` or fetched from elsewhere. Test this
  before assuming the shape.
- How much is enough, and is more actually better? A giant corpus burns context
  on every invocation. Some smaller set of well-chosen samples spanning the
  registers may beat "tons."
