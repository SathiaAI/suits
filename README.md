# SUITS — Understanding Intent Translator

**Turn what people say into what AI needs. One question max.**

SUITS (Sathia Understanding Intent Translator) is a small, model-agnostic instruction set that fixes the most common failure in human–AI collaboration: the model does the *stated* work instead of the *intended* work.

Humans compress. Someone says "sort out my kids' school stuff" or "make me a deck that lands" and holds the other 90% — what's actually overwhelming them, what done feels like, what they'd never want automated — in their head, unstated. Most assistants respond in one of two bad ways: they guess silently and build the wrong thing, or they interrogate ("What format? What length? What audience? What tone?") until the person gives up.

SUITS is the third way.

## What it does

Before any work starts, SUITS translates the raw ask into a compact **AI-Ready Brief**:

```
AI-READY BRIEF (SUITS v1.1)
Original ask: "We just bought this laundry and want to let the people in the community know"

INTENT: Win the neighborhood's first-visit trust for the new ownership — not just "post that we exist."
GOAL: Announcement materials the owners can print/post this week without further writing.
DELIVERABLE: One doc: flyer text, social post, community-group post, distribution list.
AUDIENCE: Residents within walking distance of the shop.
CONSTRAINTS: No unconfirmed claims; warm neighbor-to-neighbor tone; placeholders for unknown facts.
CONTEXT: Husband-and-wife owners; change of ownership of an existing shop.
SUCCESS CRITERIA:
1. Within 2 weeks, new customers mention "I saw your flyer/post."   ← outcome, not artifact
2. Every piece is copy-paste ready; only bracketed details to fill.
ASSUMPTIONS (veto any):
- "Community" = the physical neighborhood, not a wider digital audience [high]
- No logo/branding exists yet to reuse [med]
```

…then asks **at most one question** — the single highest-information-gain one — and only when a genuine fork exists:

> "What's the one thing that makes your place worth switching to — hours, price, new machines, drop-off service? (If you're not sure: I'll lead with 'new local owners, clean reliable machines' — the always-true hook.)"

Everything else it infers, **declares as a vetoable assumption**, and moves on.

## The core rules

- **One question per response, maximum.** If you can only ask one, you're forced to find the one that matters. Candidate questions are ranked by how much the answer changes the work × the cost of guessing wrong × whether it's truly unknowable from context. Format and length questions almost never win; audience, success definition, and scope almost always do.
- **Declared assumptions, never silent ones.** Guessing out loud is cheap; guessing silently costs the truth; interrogating costs the user. Every inference is labeled with confidence and can be vetoed before work starts.
- **The assumption boundary.** Assumptions may only cover reversible, cheap-to-redo choices. Anything irreversible, financial, external-facing, or privacy-touching is never assumed — it's either the one question, or a stop.
- **Outcome-first success criteria.** At least one criterion must verify a change in the user's world ("three neighbors mention the flyer"), not just the artifact ("flyer looks good"). When users can't articulate the outcome — they usually can't — SUITS climbs the "so-that" ladder from their words and proposes it for them to veto. People are poor specifiers and excellent critics.
- **The probe principle.** When intent stays opaque, don't ask more questions — build the smallest concrete draft and let corrections do the specifying. Iteration is elicitation.
- **Interview mode (Level 2), strictly opt-in.** If the user *asks* to be questioned ("interview me", "ask me whatever you need"), SUITS runs a disciplined interview: one question per round with a recommended answer, dependency-ordered, with a visible ledger of open forks and an always-available exit ramp ("enough — proceed") that converts whatever's left into labeled assumptions. Uninvited, it never does this.

## Use it with your model

| Platform | How |
|---|---|
| **Claude** (Claude Code, desktop, claude.ai) | Install `SKILL.md` as an Agent Skill: drop the `suits/` folder into your skills directory (e.g. `.claude/skills/suits/`), or import it via your client's skills/capabilities settings. |
| **ChatGPT** | Paste the contents of `PROMPT.md` into a Project's or custom GPT's instructions, or at the top of a conversation. |
| **Gemini** | Paste `PROMPT.md` into a Gem's instructions. |
| **Any other LLM / local model** | Prepend `PROMPT.md` as the system prompt. |

`SKILL.md` is the richer version (with reasoning, examples, and anti-patterns) for agents that support skill files. `PROMPT.md` is the same contract compressed for direct system-prompt use.

## Evals

`evals/evals.json` contains six behavioral test prompts with expected outcomes, including the two guardrail cases that matter most: a trivially clear task (SUITS must add **zero** ceremony and just do it) and an explicit "interview me" request (SUITS must ask **one** question per round with a visible ledger — not a seven-question form). In our runs, a stock assistant asked 4–7 questions on the vague prompts; with SUITS it asked exactly one, and the one that mattered.

## Why "SUITS"

SUITS is a public piece of [Sathia](https://sathia.ai) — *"Just say it."* Sathia's founding idea is that the next adoption wave for AI won't come from people learning to prompt better, but from systems that carry the burden of understanding. SUITS is that idea, small enough to paste anywhere.

## License

MIT — see [LICENSE](LICENSE).
