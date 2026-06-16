# 04 · Delegation & the Leash: How Much Autonomy to Give an Agent

> The practical question is never "should I use the agent" but "how long a leash." Set it too short and you do most of the work yourself; too long and you inherit a pile of autonomous decisions you didn't sanction. The corpus's headline rule is "short leash for taste, long leash for functional" — but the *real* variable underneath is **verifiability**.

*Part of the [Coding with AI wiki](index.md). Prev: [03 · Context Engineering](03-context-engineering.md) · Next: [05 · Verification, Review & Testing](05-verification-review-and-testing.md)*

---

## "Leash length" is the axis that actually matters

When comparing agentic tools, the useful dimension is **how long the agent works independently before it asks for feedback** — from tab-complete (Copilot finishes a line, then pauses) to fully autonomous (Devin runs for hours). This framing, attributed to Nabeel Hyatt (Spark Capital), recurs throughout the corpus and is more useful than capability claims or brand.

Guillermo Rauch ties leash length to task type:

> "It very much depends on the task… when you give us a prompt [in v0], we have to give you feedback right away because you're almost in the taste-making part of the job." — Guillermo Rauch, *Vercel's Guillermo Rauch on the Future of Coding* (2025-02-05)

The rule of thumb: **short leash for taste/design work** (you must react to each output), **longer leash for functional work with verifiable outcomes** ("make this animation faster" — success is checkable without your having been present).

## The deeper variable is verifiability, not task type

Look closely and every justification for a longer leash appeals not to *what kind* of task it is but to *whether you can check the result without having been present*. Taste work gets a short leash because you can't write a pass/fail test for "does this feel right." Functional work gets a long leash because you can: "if the agent got the Grafana dashboard running, it answers correctly; if not, it can't." (Scott Wu's eval example, *Cognition's CEO on What Comes After Code*, 2025-09-24.)

This reframe matters because it dissolves the **hybrid-task problem** the task-type rule can't handle. Don't classify the *task* as taste-or-functional — decompose it into **checkable and non-checkable sub-outcomes**:

- Run the checkable parts (does it build? do tests pass? did the form send?) on a **long leash with behavioral evidence**.
- Keep the non-checkable parts (does the interaction feel right?) **synchronous**.

And you can *manufacture* verifiability to earn a longer leash — the binary-criteria and judge-building methods in [05 · Verification](05-verification-review-and-testing.md) are, in effect, ways to make "is this good?" checkable, which lets you safely lengthen the leash on work that looked un-delegatable.

## Sync → async → sync, not one long run

Scott Wu (Cognition/Devin) models real workflows as cycles, not a single setting:

> Flesh out requirements and edge cases **synchronously** ("the Jarvis experience of talking live with your agent") → build a clear spec → hand off implementation and testing **asynchronously** → return **synchronously** for PR review and diff-reading.

He is candid that the handoff itself is unsolved: how to move between sync and async is "a pretty unanswered question today, frankly." The sync phases are where judgment lives; the async phases are where the agent earns its autonomy. Forcing the agent async during requirements is a mistake; forcing yourself sync during implementation is a waste.

Hyatt's two tactical prompts for keeping a long run honest: open with "**if anything's unclear, ask me questions before starting**," and inject "**give me five ways to solve this**" every second or third prompt to prevent premature lock-in on a wrong approach. And re-evaluate your whole calibration roughly every six months, because the right leash length shifts as models improve.

## "Yolo mode," automations, and the long end of the leash

For functional coding work, several builders default to **auto-accept** ("yolo mode") and evaluate at completion, not mid-run. Danny Aziz (Spiral) runs multiple sessions with auto-accept on; his standard is not "is the code good?" but "does the thing Claude produced do what I wanted, the way I wanted it?" Interrupting flow for intermediate review defeats the point of parallel async sessions.

The longest leash is **scheduled automation** — recurring agent runs with no human initiation. The Codex team runs ~60 automations: keep open PRs rebased and building, daily digests of merged work, and self-improving loops that pick a *random* file and fix a subtle bug, or "try to ship a fix before anyone's noticed that you shipped a bug." (*OpenAI's Codex This Model Is So Fast*, 2026-02-18). The condition: the work must have a success state that can be verified post-hoc — maintenance qualifies; taste and strategy do not.

Andrew Wilkinson's version: "I just popped off a bunch of Claude Codes on my phone to do 15 different [things]… I spun up a bunch of parallel things to fix little bugs" at breakfast — the bottleneck being testing/closing the loop, not generation. (*How Andrew Wilkinson Uses Opus 4.5*, 2026-01-21)

## The "abundance mindset" for delegation agents

Codex's design leans the opposite way from plan-first discipline — toward firing many tasks without over-specifying:

> "They just fire off many, many tasks without thinking too hard about whether it's going to work… you don't need to know [the bug], you just fire it and sometimes it's 'here's the exact fix.'" — Alexander Embiricos, *OpenAI Launches Codex* (2025-05-16)

This is reconcilable with plan-first (see [02](02-planning-and-specs.md)): abundance suits **delegation agents that return reviewable diffs asynchronously** (a failed attempt is just a diff you don't merge); plan-first suits **agents that execute live in your environment** (a wrong run must be reverted). The right level of up-front specification tracks the *cost of a wrong execution*.

## Delegation is a management skill — and most people under-delegate

The recurring claim: using agents well is people-management, not a technical trick.

> "If you're not a good manager, you've never managed anybody, you're not going to be very good at using AI." — Dan Shipper, *We Gave Every Employee an AI Agent* (2026-04-08)

People who've never managed carry "limiting beliefs" about what an agent can do and never delegate far enough to discover its real limits. Brandon's example of breaking one: he had his agent *phone him* and walk through his inbox one by one during a 28-minute walk — "that's when a limiting belief was blown open." Outcome variance is high and partly a *you* problem: "if I'd asked in a different way, if I was a better model manager."

The unifying principle for *where* a human stays attached: **"The further away an agent gets from a human, the less valuable it is."** (Dan Shipper). Autonomy is good; detachment from human judgment is not.

## Speed can collapse async back into sync

A non-obvious twist: as models get fast, the async/long-leash advantage can *invert*. The Codex 2026 team found that once the model is fast enough, "I'm able to do a little bit less multitasking and be more in the flow" — and "you cannot delegate understanding, so speed there is a real advantage." Long leash is not monotonically better; raw speed can make staying in the loop the better choice. (See [09 · Choosing Models](09-choosing-models-for-coding.md).)

## Failure modes at the long end

- **Vague + long = breakage.** Devin "breaks a lot" on vague or complex problems; "set the leash too long for most current use cases unless the problem is very specific." (Hyatt)
- **Agent-to-agent death spirals.** Put several agents in one Slack/Discord channel and they over-contribute and enter token-burning loops, because models are trained on two-person conversations, not group-chat etiquette. Prompting ("don't contribute unless useful") helps but isn't enough. (*We Gave Every Employee an AI Agent*) — see [07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md).
- **Cost risk.** Under usage-based pricing, a poorly-scoped autonomous run can burn real money; the corpus notes the risk but offers no cloud-cost-monitor equivalent.

## Tensions & open questions

- **Ask-first vs. yolo as the default opening.** Hyatt front-loads clarification; Aziz starts and evaluates at the end. Likely conditioned on task complexity and verifiability, but the corpus doesn't draw the line cleanly.
- **When is a long run too long to trust?** No concrete threshold is offered beyond "breaks a lot on vague problems."
- **Agent-to-agent trust** is unsettled. One workable lightweight rule: require agent interactions to happen in *public* channels so the human owner keeps visibility (see [07](07-parallelism-and-agent-fleets.md)).

## How to apply it

- Set the leash by **verifiability**: decompose the task, run checkable parts long-and-async, keep non-checkable parts short-and-sync.
- Open long runs with "**ask me clarifying questions first**"; periodically ask for **several options** to avoid early lock-in.
- Reserve **automations** for maintenance work with a clear post-hoc success state.
- Treat delegation as **management**: notice your own limiting beliefs and push the leash out to find the real edge.
- Re-calibrate every few months, and remember a *faster* model may be a reason to stay *closer*, not further.

## See also

- [05 · Verification, Review & Testing](05-verification-review-and-testing.md) — verifiability is the variable behind the leash; how to build the checks
- [02 · Planning & Specs](02-planning-and-specs.md) — plan-first vs. abundance, reconciled
- [07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md) — running many agents, and agent-to-agent failure modes
- [09 · Choosing Models for Coding](09-choosing-models-for-coding.md) — how speed and capability move the right leash length

## Sources

Primary: *Vercel's Guillermo Rauch on the Future of Coding* (2025-02-05); *Cognition's CEO on What Comes After Code* (Scott Wu, 2025-09-24); *OpenAI Launches Codex* (Alexander Embiricos, 2025-05-16); *OpenAI's Codex This Model Is So Fast* (Tibo Sottiaux / Andrew, 2026-02-18); *We Gave Every Employee an AI Agent* (Dan Shipper / Willie / Brandon, 2026-04-08); *How Andrew Wilkinson Uses Opus 4.5* (2026-01-21). Nabeel Hyatt's leash-length framing and Danny Aziz's yolo-mode pattern are referenced as they recur across the corpus.
