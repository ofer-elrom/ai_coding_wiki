# 03 · Context Engineering

> "Every problem becomes a context problem." Most agent failures are not the model being dumb — they are the model missing the right information, or drowning in the wrong information. Managing what's in the context window is the new core discipline of coding with AI.

*Part of the [Coding with AI wiki](index.md). Prev: [02 · Planning & Specs](02-planning-and-specs.md) · Next: [04 · Delegation & the Leash](04-delegation-and-the-leash.md)*

---

## The thesis

Matt Colyer (Figma) states it plainly: "Every problem becomes a context problem… the work becomes about framing the problem with the right set of information." (*The SaaS Apocalypse Is a Goldmine*, 2026-06-03). Mike Taylor puts the failure side just as bluntly: "It almost always comes down to what was in the context window at the time that you asked it to do that." (*What Happens When Beginners Start Building With Claude Code*, 2026-03-17)

The skill has two opposite failure modes, and the craft is balancing them:

- **Too little context** → the model guesses, and "when it's filling in these details, sometimes it goes rogue." (Kieran Klaassen)
- **Too much context** → **context rot**: "the more information you put into that chat session, the more likely it is to go off the rails." (Mike Taylor)

## `CLAUDE.md` as a job description — lean, not a knowledge dump

The most-discussed primitive is the persistent config file the agent reads every session (`CLAUDE.md`; `AGENTS.md` for Codex). The consensus is that it should be a **job description**, not an encyclopedia.

Natalia (Every), describing "Claudie," their AI project manager built on Claude Code:

> "Opus 4.5 is really, really smart. It actually doesn't need us to define what a project manager is… but these boundaries, conventions, and refining of sharp edges have really allowed Claudie to do really good work." — Natalia, *How We Built 'Claudie'* (2026-02-04)

What goes in it: who the agent works with, where data lives, naming conventions, operating principles ("proactive not reactive," "when in doubt, escalate," "every interaction builds or erodes trust"), and the data-ID scheme. The maintenance habit, from Anthropic's own team: when the agent makes a mistake, "at Claude, add this to CLAUDE.md" — encode why it was wrong, the way you'd correct a new employee. Reserve it for things true on *every* task; situational fixes go elsewhere (next section).

## Don't dump everything in one file — use learnings files + a retrieval sub-agent

The counter-discipline, from the Compound Engineering plugin, is that a growing monolithic file *is* the context-rot problem:

> "If you would add all of this information to a [CLAUDE].md it will just grow insanely… the beauty is this document is never injected inside the prompt." — Kieran Klaassen, *Compound Engineering: Your Questions Answered Live* (2026-02-24)

Instead: store learnings as **individual markdown files with front-matter metadata** in a folder structure (brainstorms, investigations/postmortems, solutions, best-practices). A `best-practice-researcher` **sub-agent**, built into the plan and work commands, searches those files and passes only relevant summaries into the main context — the full documents are never auto-loaded. "It's a sub-agent so it doesn't waste the context. It just finds the document and passes the learning."

The reconciling rule between the two camps: **`CLAUDE.md` holds always-relevant project context; learning files hold situation-specific knowledge retrieved on demand.** Maintaining that boundary requires ongoing judgment — see [06 · Compounding Engineering](06-compounding-engineering.md) for the full machinery.

## Sub-agents earn their keep through *uncorrelated context windows*

Boris Cherny demystifies the popular "frontend dev / backend dev / PM" sub-agent personas:

> "It feels a little too anthropomorphic… the value is actually the uncorrelated context windows… you tend to get better results this way." — Boris Cherny, *Inside Claude Code* (2025-10-29)

A fresh sub-agent doesn't inherit the biases and clutter of the parent conversation, so it reasons more cleanly on a focused job. Mike Krieger adds the corollary: when a prompt gets bloated, don't keep appending instructions — "ask, are these actually two different tools? Is it actually two agents that each have a smaller amount of context?" Onboarding analogy: give a new hire 100 first-day instructions and "they'll just remember the last thing you told me." (*Building Is the Easy Part Now*, 2026-03-25)

## Unique IDs: context engineering as data modeling

A concrete, under-appreciated unlock for agents that relate information across sources: give every entity an ID.

> "Creating ID conventions that are effectively like database management were a huge unlock for us to have this entire system work well." — Natalia, on Claudie

Once people, teams, and sessions each have IDs, the agent can do relational lookups across email, notes, and Drive without guessing at identity. This is a *design* decision in how you structure the agent's world, not something you prompt at task time.

## Prefer connectors over content; assume context goes stale

A hard, recurring problem: shared business/strategy context rots. Mike Taylor calls keeping it fresh "the million-dollar question nobody has solved," and offers the best available heuristic:

> "You only really know that the context is stale when AI makes a mistake… one thing you can do is aim for connectors over content." — Mike Taylor

Live connectors (MCP servers, read-only DB replicas) beat static documents that silently drift. Caveat: connectors come with their own paper cuts — "the MCP servers disconnect and then I get annoyed" (Alex Rattray, *MCP Servers*, 2025-10-01) — and naïvely wiring a full API into context blows the budget (see below and [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md)).

## Reactive, lightweight context rules: `.cursorrules` and notepads

Kieran's day-to-day pattern in Cursor mirrors the `CLAUDE.md` discipline at a smaller grain:

- **Cursor rules** (`.cursorrules`): "always follow this." Populated reactively — "when I see things go wrong, I just go in here and edit… so I don't have to say that every time."
- **Notepads**: "use it in specific cases where I specifically ask" — on-demand context, not always-on.

The general move: every time you find yourself typing the same correction twice, promote it into a rule. (More on this compounding loop in [06](06-compounding-engineering.md).)

## The context budget is a hard, quantitative constraint

For agents talking to external systems, context is a literal budget. Alex Rattray (Stainless, ex-Stripe API team):

> "You've just burned through your entire context budget… maybe hundreds of thousands of tokens just there… it's also confusing to the model." — on naïvely mapping a full API (Stripe's hundreds of endpoints) into MCP tools.

His fixes (detailed in [10](10-agents-as-users-and-infrastructure.md)): few, hand-crafted tools; trim responses with a `jq` filter; and the bigger move — **code execution**, where pagination/loops happen server-side in a CPU and only the final `console.log` returns ("the context hit coming back is like 10 lines"). Memory and context windows that "run out" are a live pain point even in frontier models (Opus 4.6 was reported to be token-hungry and to fail to auto-compact on overflow — see [09](09-choosing-models-for-coding.md)).

## Memory is the real bottleneck for delegation

Kevin Scott (CTO, Microsoft) names what's missing for longer-horizon work:

> "[Memory today is transactional]… it may or may not completely go away, and you're starting from scratch the next time, which really inhibits your ability to delegate increasingly complicated tasks." — Kevin Scott, *Kevin Scott on the Future of Programming* (2025-05-21)

An emerging pattern from Anthropic's team: rather than one-shot instruction memory (which over-generalizes — "make all buttons pink" shouldn't become a rule), some engineers have Claude keep a per-task diary, with a separate agent that "synthesizes it into observations." Flagged as promising but unproven.

## Tensions & open questions

- **`CLAUDE.md` as home for everything vs. dedicated learning files.** The Anthropic-team habit ("add it to CLAUDE.md") and the Compound-Engineering "never bloat CLAUDE.md" position only reconcile if you keep the always-relevant / situational boundary — which itself takes judgment.
- **Context compaction can destroy the moment of learning.** The best time to encode a lesson is right after it happens, while context is full — but automatic history-summarization can erase the precise context you needed. No one offers a clean fix.
- **How do you share compounded context across a team?** Flagged repeatedly as unsolved (a Notion "compound database," plan-sharing via Proof). "Cannot be real time either, because it's too slow." (Kieran)

## How to apply it

- Keep `CLAUDE.md`/`AGENTS.md` **short and job-description-shaped**; push situational knowledge into searchable learning files retrieved by a sub-agent.
- Treat **too much context as a real risk**, not just too little. Start fresh sessions before context rot sets in.
- Use **sub-agents to isolate context** for focused jobs; split a bloated prompt into multiple smaller-context agents.
- Give the entities your agent reasons over **stable IDs**.
- Prefer **live connectors over static docs**, and remember you only learn context is stale when the agent errs.
- Encode every repeated correction into a **rule** the moment you notice it.

## See also

- [06 · Compounding Engineering](06-compounding-engineering.md) — the system that captures and retrieves learnings
- [07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md) — sub-agents and uncorrelated context at scale
- [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md) — MCP, code execution, and the context budget
- [05 · Verification](05-verification-review-and-testing.md) — silent failures that are really context gaps

## Sources

Primary: *The SaaS Apocalypse Is a Goldmine* (Matt Colyer, 2026-06-03); *What Happens When Beginners Start Building With Claude Code* (Mike Taylor, 2026-03-17); *How We Built 'Claudie'* (Natalia, 2026-02-04); *Compound Engineering: Your Questions Answered Live* (Kieran Klaassen, 2026-02-24); *Inside Claude Code* (Boris Cherny / Cat Wu, 2025-10-29); *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *MCP Servers Teaching AI to Use the Internet Like Humans* (Alex Rattray, 2025-10-01); *Kevin Scott on the Future of Programming* (2025-05-21); *He Built an AI App With 10,000 Signups* (Kieran Klaassen, 2025-01-15).
