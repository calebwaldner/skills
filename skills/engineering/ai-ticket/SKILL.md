---
name: ai-ticket
description: Triage an AI-written ticket — separate the problem from the generated proposal, verify both against the code, and recommend an approach before anything is built.
argument-hint: "Paste or link the ticket, plus anything you already know about it"
disable-model-invocation: true
---

# AI Ticket

A ticket written by an AI arrives with two things fused together: the **problem** a human wanted solved, and a **proposal** for how to solve it that the AI generated on the way out. The proposal was produced without this codebase's conventions, without the history of why things are the way they are, and without accountability for the result. That is why it reads here as a proposal and never as instructions. A human-written ticket is different: its implementation notes carry the author's knowledge of the system and are read with full weight. Caleb can tell the two apart and you cannot, so this skill fires only when he invokes it, and only on an AI-written ticket.

The proposal still earns its keep. It is a map of where the ticket's author believed the problem lives, and a map is the fastest way to start looking. The job is to use the map, verify the territory, and bring Caleb a recommendation he can decide on. The decision is made together, and usually that means a `/grilling` session running beside this skill (named at boot as `grill-with-docs` or `grill-me`): the grill is where the decision gets made, and this skill decides what the grill treats as open. Nothing is built here.

## Steps

1. **Separate.** Split the ticket into the problem and the proposal. The problem is restated in one or two sentences as a symptom or a need, the way a user or the system would see it, with no fix implied. The proposal is every statement about *how*, listed apart, with enough of the original wording to evaluate later. Acceptance criteria usually belong to the problem, but one that names a component, endpoint, or approach is proposal in disguise and goes to that list. Done when the problem statement contains no file, function, component, or approach, and everything of that kind sits in the proposal list.
2. **Verify.** Trace every claim in the problem statement to the code or to a reproduction. Start where the proposal points, since that is where the author's AI thought the problem lives, then look where it did not point. Done when every claim is marked **confirmed**, **contradicted**, or **unverified**, with where you looked. A problem that is contradicted, or that cannot be verified at all, is the finding: report that and stop, because there is nothing yet to evaluate approaches against.
3. **Evaluate.** List the candidate approaches with the ticket's proposal as one of them, judged on the same terms as the rest. Judge each against what verification showed: fit with how the surrounding code already solves this kind of thing, blast radius, what it leaves unsolved. Done when every candidate carries a verdict with its reasoning, and the proposal's verdict names what the AI could not see from outside the codebase, whether that helped it or hurt it.
4. **Recommend.** Two branches, by whether a grill is running in this session:
   - **Paired with `/grilling`** (the usual case). The recommendation is the grill's opening round, not a report. The design tree's root is the restated problem, never the ticket: Q1 puts the problem to Caleb for confirmation, since the AI may have misread the human's intent and Caleb knows the human. Every item in the proposal list is an *open* decision on the tree, even where the ticket wrote it as settled, and goes to Caleb as a numbered question with the ticket's own suggestion as one of the choices and your evaluated verdict as the ➡️ recommended answer. These ride in round 1 beside Q1 rather than waiting on it, because verification already grounded them in the code. Verification results are the facts the grill says you find yourself, so they are in hand before the first question. Done when round 1 is on the table and every proposal item appears in it, or sits behind a dependency that will surface it in a later round.
   - **Alone.** Fill the template below and hand it to Caleb. Done when he has one recommendation with its reasoning and the tradeoffs, and no code has changed. Implementation happens after agreement, through whatever he picks next (`/work-local`).

## Rules

- **The proposal is evaluated.** It is the AI's best guess from outside the system, and the judgment this ticket exists to get is whether that guess survives contact with the code. Evaluating it is the work; executing it skips the work.
- **The proposal is a map.** Its real value is directional: it says where to look first. Use it to locate, then verify what is actually there, and keep looking where it did not point.
- **The proposal can win.** This skill is independent, not contrarian. When the evaluation lands on the ticket's own approach, say so plainly and recommend it. Rejecting it by reflex is the same failure as following it by reflex: the code did not decide.
- **Human words on an AI ticket keep their weight.** A comment, note, or edit a person added to the ticket is the author's knowledge of the system. It belongs with the problem and the context, never in the proposal list, and it is read with full weight.
- **Nothing is built here.** The skill ends at the recommendation, or hands it to the grill, which has its own gate. An agent that has already started building defends what it built, and "together" means the decision happens before that point.

## Template (alone branch)

```markdown
## Problem
<one or two sentences, no fix implied>

## Proposal (from the ticket)
- <each statement about how, as the ticket put it>

## Verification
| Claim | Result | Where I looked |
|---|---|---|
| <claim from the problem> | confirmed / contradicted / unverified | <file, reproduction, or note> |

## Candidates
1. <approach> — <verdict and why>
2. <the ticket's proposal> — <verdict; what the AI could not see from outside>

## Recommendation
<one approach, the reasoning, the tradeoffs Caleb is deciding between>
```
