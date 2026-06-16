# 07 · Parallelism & Agent Fleets

> When generation is cheap and agents run asynchronously, the natural unit of work stops being "one agent, one task" and becomes "many agents at once." This doc covers running fleets in parallel, why sub-agents help, the orchestration patterns that work, and the genuinely unsolved problems of agents working alongside each other.

*Part of the [Coding with AI wiki](index.md). Prev: [06 · Compounding Engineering](06-compounding-engineering.md) · Next: [08 · Tools, Harnesses & Form Factors](08-tools-harnesses-and-form-factors.md)*

---

## Many agents at once is the new normal

The Cora team runs "six or seven Claude Code sessions in parallel" during a brainstorm — "new idea, let's go; new idea, let's go" — and shipped a feature to production live during a recording without touching the codebase. Andrew Wilkinson fired off ~15 Claude Code tasks from his phone at breakfast. GitHub's coding agent runs ~10 in parallel on GitHub Actions, each returning a PR.

The enabling mindset is **delegation, not flow-state coding**. Yohei Nakajima: "I choose when my code needs me… I can be gone for 20 minutes and come back and you'll still be exactly where I thought you'd be — it's exactly how a manager operates." Spinning up several agents needs only "the attention of when a coworker pings you on Slack." (See [04 · Delegation & the Leash](04-delegation-and-the-leash.md).)

This also produces **"social coding"** — two developers on a video call, each driving multiple agents, chatting while work ships: "work is getting done, which I've never really experienced before… a new model for building things." (Embiricos, *OpenAI Launches Codex*, 2025-05-16)

## Sub-agents: it's about uncorrelated context, not role-play

The most important technical point about fleets, from Boris Cherny:

> "[The frontend-dev/backend-dev/PM personas feel] a little too anthropomorphic… the value is actually the uncorrelated context windows… you tend to get better results this way." — *Inside Claude Code* (2025-10-29)

A fresh sub-agent reasons cleanly because it doesn't inherit the parent conversation's clutter and biases. The standard heavy pattern is **map-reduce over ~10 sub-agents** for large migrations, followed by a **dedup / false-positive pass**:

> A power-user command spawns sub-agents (a CLAUDE.md-compliance check, a git-history check, a bug check), "then we spawn five more sub-agents all just checking for false positives… it finds all the real issues without the false issues." — Boris Cherny

Classic targets: rolling out a lint rule with no autofixer, and test-framework migrations (easy to verify). Mike Taylor's lighter version: spin up a *fresh* session to review the first agent's code "as if an intern wrote this" (see [05 · Verification](05-verification-review-and-testing.md)).

## Orchestration patterns are Lego-like and task-specific

Anthropic's platform team frames multi-agent orchestration as composable primitives, with the right shape depending on the job:

> "If we can make the primitives very Lego-like, then people can put them together… some are much better for deep research, the ones where they all swarm together are better for bug hunting." — Caitlyn (*The Secrets of Claude's Platform*, 2026-05-08)

Named patterns: **advisor/strategy** (separate the agent that advises from the one that executes), **generator vs. adversarial**, **split-and-recombine**, and **best-of-N**. The recurring lesson is *specialization beats a single god model* — the vending-machine test where a "storekeeper" Claude only became profitable once a second agent with one job ("make it profitable") was added on top: "specialization, even in AI land, has a lot of benefit." (*We Gave Every Employee an AI Agent*, 2026-04-08)

## Sandboxes and worktrees make parallelism safe

Running many agents in parallel raises the "what container does this run in" question. Approaches in the corpus:

- **Isolated cloud containers per task** (Codex spins one up per task; internet is cut after setup for safety — see [14 · Pitfalls](14-pitfalls-limits-and-risks.md)).
- **A persistent cloud machine** the agent owns (Devin — "almost like onboarding a software engineer").
- **A sandbox with a big stop button.** Yohei runs everything in Replit specifically so that "if I see a whole bunch of red I can just push stop" — sandbox safety for "an immature developer building autonomous agents."
- Boris notes the social artifact of long runs: people "walking around with their Claude Codes," afraid to close their laptops, and "Claudes monitoring Claudes."

## Shared agent sessions: collapsing the collaboration loop

Linear demoed a step beyond parallel-but-isolated: a *shared* agent session where a PM and a designer (or two engineers) "both jump into this chat and tweak it together," seeing the same preview and PR diff. Code review can then **re-delegate** the fix to the agent instead of to another engineer. "It kind of collapses the collaboration loop." (Karri Saarinen, *How Linear Turned AI Agents Into First-class Users*, 2026-04-01)

## When agents work *alongside each other*: the unsolved part

Every gave each employee a personal agent (a "Plus One") in shared channels, and surfaced the failure modes of multi-agent social space:

- **The "ant death spiral."** With the wrong settings, agents in a shared channel reply to each other indefinitely, "burning millions of tokens" until a human says stop. Root cause: "the way these AIs are trained currently is for two-person conversations… they have a hard time with the etiquette of knowing when they're contributing too much." Prompting helps but "there's a fundamental model-layer shift that needs to happen."
- **One bad agent can pollute shared work.** From building Proof: "agents have a ton of power and zero responsibility," unlike humans who feel "a big social cost to ruining someone else's document." Agents kept "duplicating and then triplicating the page." The proposed design: a **canonical version that only updates on explicit human yes**, while agents freely mess with other versions you merge from.
- **Trust via public-only interaction.** Every's lightweight rule: anyone can message any agent, but only in *public* channels, so the human owner keeps visibility; private DMs are owner-only. This sidesteps thorny access-control and "transfers trust from person to agent via public accountability."

On the upside, **knowledge spreads through a fleet almost instantly** — one agent writes a doc, shares it, "and now five claws are all enabled with the same thing" (the Matrix "I know kung fu" moment).

## Tensions & open questions

- **Sub-agents can *slow down* fast models.** With the ultra-fast Spark model, the same review took ~1.5 min without sub-agents vs. ~4 min *with* them (context-passing + synthesis overhead), leaving only ~26% context. For tasks fast enough to watch, orchestration overhead can cost more than it saves — and some builders speculate a return to a single "mega-prompt." (See [09 · Choosing Models](09-choosing-models-for-coding.md).) Don't cargo-cult swarms.
- **Group-chat etiquette is a model-training gap,** not just a prompting problem.
- **Coordination overhead.** Mike Krieger warns against pre-scaling: while an idea fits in one head, adding people (or agents) is a net negative — "they end up spending all this time on coordination."
- **Agent-to-agent trust and identity** remain genuinely unsolved (see [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md)).

## How to apply it

- Run agents in **parallel** for independent tasks; check in like a manager rather than watching each one.
- Use **sub-agents for uncorrelated context**; for big migrations, **map-reduce then dedup** with a false-positive pass.
- Pick the orchestration shape to the job: **swarm for bug-hunting**, split-and-recombine or best-of-N for research/quality.
- Run fleets in **sandboxes with a stop button**; isolate write access.
- If agents share a space, force **public interaction**, keep a **canonical human-gated version**, and watch for **death spirals**.
- With very fast models, **measure** whether sub-agents actually help before adding them.

## See also

- [04 · Delegation & the Leash](04-delegation-and-the-leash.md) — the manager model that makes fleets viable
- [03 · Context Engineering](03-context-engineering.md) — uncorrelated context windows; splitting bloated prompts
- [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md) — agent identity, trust, and multi-agent platforms
- [09 · Choosing Models for Coding](09-choosing-models-for-coding.md) — when speed makes orchestration overhead net-negative

## Sources

Primary: *Inside Claude Code* (Boris Cherny, 2025-10-29); *OpenAI Launches Codex* (Alexander Embiricos, 2025-05-16); *The Secrets of Claude's Platform* (Angela / Caitlyn, 2026-05-08); *We Gave Every Employee an AI Agent* (Willie / Brandon / Dan Shipper, 2026-04-08); *How Linear Turned AI Agents Into First-class Users* (Karri Saarinen, 2026-04-01); *How We Use Proof* (2026-03-11); *Building AI That Builds Itself* (Yohei Nakajima, 2024-10-23); *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *Live Vibe Check: Spark* (Dan Shipper / Kieran Klaassen, 2026-02-13).
