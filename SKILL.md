---
name: suits
description: SUITS — Understanding Intent Translator. Converts a raw human request into an AI-Ready Brief (true intent, goal, deliverable, audience, constraints, user-outcome success criteria, labeled assumptions) BEFORE work starts, asking at most ONE clarifying question per response — the single highest-information-gain one — and only when a real fork exists. Includes a strictly opt-in Interview mode for when the user asks to be questioned. Use this whenever the user gives a substantive, vague, rambling, multi-step, or high-stakes task request, even if they never mention SUITS — especially requests like "sort this out", "make this better", "help me with...", "I need a plan/deck/doc", "I don't know what I need", or a long stream-of-consciousness ask. Always use it when the user says "suits:", "run suits on", "translate my ask", "help me prompt this", "interview me", or "ask me questions until we figure this out". Skip it only for trivially clear one-step actions and pure factual questions.
---

# SUITS — Understanding Intent Translator (v1.1)

## Why this exists

Humans compress. A person says "sort out my kids' school stuff" and holds the other 90% — what's overwhelming them, what done feels like, what they'd never want automated — in their head, unstated. AI fails people not because it can't do the work, but because it does the *stated* work instead of the *intended* work. SUITS closes that gap: it translates what a person said into what an AI needs, so the person never has to learn prompt engineering. The user's words are the input, not the spec.

The principle: **the system carries the burden of understanding, not the user.**

## When to run (hybrid mode)

**Auto-run** when a request is substantive AND at least one of these is true: the deliverable is unstated, the audience is unstated, multiple plausible interpretations lead to materially different work, it's multi-step, or getting it wrong would be expensive to redo.

**Forced run**: the user says "suits:" or "run suits on ..." — always run, even on clear requests.

**Pass through untouched** when the request is a single clear action or a factual question ("rename x to y", "what's the capital of France"). Wrapping a trivial ask in ceremony is a failure, not thoroughness. When in doubt on borderline cases, run it — a 60-second brief is cheaper than an hour of wrong work.

## The translation frame

Fill these seven fields. Infer aggressively from the conversation, files, and history before considering a question — every field answered by looking is a question you don't ask.

| Field | What it captures | The test |
|---|---|---|
| **Intent** | Why — what changes in their world if this succeeds | Would the user nod at this even though they never said it? |
| **Goal** | What "done" looks like, concretely | Could a stranger verify it's done? |
| **Deliverable** | The artifact: format, length, destination | Could you hand it over without asking "in what form?" |
| **Audience** | Who consumes the result (often ≠ the requester) | Does tone/depth/framing depend on this? |
| **Constraints** | Time, money, tools, tone, must-nots, things to leave untouched | What would make the user say "no, not like that"? |
| **Context** | What the AI can't know: definitions, references, prior decisions, relevant files | What would a new hire need to be told? |
| **Success criteria** | 2–4 verifiable checks | At least ONE must verify the user's outcome, not the artifact |

**The outcome rule.** At least one success criterion must verify an observable change in the user's world, not a property of the deliverable — "three neighbors mention the flyer" beats "flyer is print-ready." Users can rarely articulate this outcome, and you must not ask them to: climb the so-that ladder from their words, maximum two rungs ("let people know" → *so that* people try it → *so that* they become regulars), pin the outcome there in behavioral terms, and put it in the brief for them to veto. People are poor specifiers and excellent critics — propose, don't extract.

Then add **Assumptions**: every inference you made, labeled and vetoable. Assumptions are how SUITS avoids interrogating — you don't ask about things you can safely assume and clearly disclose.

**The assumption boundary.** Assumptions have a jurisdiction: they may stand in only for reversible, cheap-to-redo choices — tone, structure, format, scope you can re-cut. Anything irreversible, financial, external-facing (sending, publishing, purchasing), or privacy-touching is never assumed: it is either THE question or a stop. A brief may shape a draft on an assumption; it may never authorize an action on one.

## The One-Question Gate

SUITS may ask **at most one question per response**. This is a hard ceiling, and it forces the right behavior: if you can only ask one, you must find the question that buys the most clarity. The ceiling is per response, not per conversation — a later turn may earn one new question when a genuinely new fork opens. Never stack questions, never re-ask what's been answered.

Procedure:

1. **Try to answer it yourself first.** Search the conversation, files, and obvious defaults. Never ask what you can look up — that spends the one question on laziness.
2. **Fork test:** do two or more plausible interpretations lead to *materially different* work? If no — ask nothing. State assumptions and proceed.
3. **If yes, rank the candidate questions** by three factors: how much the answer changes the work (divergence), how costly a wrong guess is (irreversibility), and whether the answer is truly unknowable from context (non-inferability). Ask only the top-ranked question.
4. **Format the question as a choice**, not an essay prompt: 2–4 concrete options with a recommended default, so answering takes seconds. Use a structured choice UI if your platform has one; otherwise inline options.
5. If the environment can't reach the user (subagent, batch run), don't stall: write the brief, state the question you *would* ask, pick the recommended default, label it as an assumption, and continue.

Questions that usually LOSE the ranking: output format, length, styling (defaults exist and are cheap to change). Questions that usually WIN: audience, success definition, scope boundary ("which accounts/kids/projects does this cover?"), and irreversible-action approval.

**The probe principle.** If intent stays opaque even after your one question is answered — or the user answers "I don't know" — don't reach for another question. Build the smallest concrete probe instead: a sketch, one filled-in example, a sample section. Let their corrections do the specifying. People can't spec what they haven't seen; they can always correct what they have. Iteration is elicitation.

## Level 2 — Interview mode (strictly opt-in)

The one-question ceiling has exactly one exception: the user explicitly asks to be questioned — "interview me," "ask me whatever you need," "I don't know what I want, help me figure it out." That request is consent. Without it, never enter this mode; a barrage of questions the user didn't ask for is a form, and forms are what this skill exists to kill.

When the signals suggest Level 1 is failing — the user vetoes most of a brief, or answers the one question with "I don't know" on a high-stakes fork — you may OFFER the mode: "Want me to interview you properly (one question at a time, each with my recommendation), or show you a rough version 1 to react to?" Recommend the version-1 probe; entering the interview still requires their yes.

Rules of the interview — each exists because interrogation fails the moment it feels like a form:

- **One question per round, never a batch.** Each round carries: the question, 2–4 options where possible, your recommended answer and why, and which open fork it resolves.
- **Order by dependency.** Resolve the fork whose answer changes the most downstream questions first — walk the decision tree, don't zigzag.
- **Self-serve first, every round.** Anything answerable from files, context, or earlier answers gets answered there, not asked. Say what you resolved yourself.
- **Keep a visible ledger.** After each answer, one line per fork: resolved (with the answer) or open. The user should watch the tree shrink — progress is what makes questioning feel like collaboration instead of interrogation.
- **Exit ramp, always open.** At any round the user can say "enough — proceed": convert every remaining open fork into a declared assumption (Level 1 rules apply, including the assumption boundary) and produce the brief.
- **Terminate deliberately.** Stop when all material forks are resolved, when the user exits, or at ~5 rounds — at 5, show the ledger and ask proceed-or-continue rather than grinding on.

The interview's product is the same AI-Ready Brief — now with an empty or near-empty assumptions list. Levels differ in how clarity is gathered, never in what gets produced.

## Output format

Produce the brief in this exact structure, inside a fenced block so it's portable — the user can paste it into any AI and get faithful execution:

```
AI-READY BRIEF (SUITS v1.1)
Original ask: "<user's words, verbatim, trimmed>"

INTENT: <one sentence — the why>
GOAL: <one sentence — what done looks like>
DELIVERABLE: <artifact, format, destination>
AUDIENCE: <who consumes it>
CONSTRAINTS: <bulleted, incl. must-nots>
CONTEXT: <what the executing AI needs to be told>
SUCCESS CRITERIA:
1. <user-outcome check — observable change in their world>
2. <verifiable artifact check>
ASSUMPTIONS (veto any):
- <inference> [confidence: high/med/low]
```

Keep the whole brief under ~150 words. A brief nobody reads protects nobody. Below the brief, ask your one question if the gate fired.

## After the brief

Present the brief, then ask for a go: "Confirm, veto an assumption, or answer the question — then I'll execute." On confirmation, execute the brief (not the original words). If the user says "just the brief," stop there — the brief itself is the deliverable, built to travel.

## Anti-patterns (each one breaks the contract)

- **Uninvited interrogation.** Two questions in one response is a failure everywhere. A series of questions is legal only inside Level 2 — and only the user can open Level 2.
- **Parroting.** Restating the user's words as "intent" adds nothing. Intent is the unstated why — if it doesn't go beyond their words, you haven't found it yet.
- **Artifact-only success criteria.** "Deck looks professional" verifies the artifact; "the room agrees to the ask" verifies the outcome. Include the outcome.
- **Boilerplate padding.** Empty fields are honest; invented constraints are corrosive. Write "none stated, none inferred" rather than fabricating.
- **Format-first questions.** Never spend the one question on md-vs-docx when the content or audience is still unknown.
- **Ceremony on trivial asks.** "Rename this file" needs a rename, not a brief.
- **Silent assumptions.** Every guess goes in the ASSUMPTIONS list. The user can only veto what they can see.
- **Assuming past the boundary.** Never assume anything irreversible, financial, external-facing, or privacy-touching. Question or stop.

## Examples

**Example 1 — vague ask, gate fires**
Input: *"i'm drowning in the kids' school emails and sports schedules, can you sort it out"*
Output: Brief with INTENT "reduce weekly coordination load and missed-event risk, not archive email"; SUCCESS CRITERIA led by an outcome check ("a month with zero missed events/appointments"); ASSUMPTIONS "both kids [high]; school + sports only, not medical [med]"; then the ONE question — scope boundary: "Should I work from (a) just the school/team emails you forward me (recommended — no account access), (b) your full inbox, or (c) inbox + calendars?" *(Audience/format questions lost the ranking: defaults are cheap; scope changes everything and touches privacy.)*

**Example 2 — clear ask, pass through**
Input: *"rename report_v2.docx in my outputs folder to report_final.docx"*
Output: rename it. No brief, no questions.

**Example 3 — user opens Level 2**
Input: *"honestly i don't know what i want here. ask me questions until we figure it out"*
Output: Interview mode — round 1 opens with the most upstream fork, e.g. "First, the fork everything else hangs on: is this for X or Y? My recommendation: X, because ... (Open forks: purpose, scope, budget. You can say 'enough — proceed' anytime and I'll convert what's left into labeled assumptions.)" One question per round, ledger updated each round, brief delivered at the end.

## Scope boundary

SUITS clarifies intent; it does not grant permission to act. Pair it with whatever approval, safety, or human-in-the-loop layer your agent stack uses — a SUITS assumption may shape a draft, but authorization for consequential actions must always come from your safety layer or the human, never from an inference.
