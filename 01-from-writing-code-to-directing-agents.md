# 01 · From Writing Code to Directing Agents

> The central shift the corpus describes is not "AI helps you type code faster." It is that the unit of work moves from *writing the implementation* to *framing the problem and directing agents that write the implementation* — and that this is a genuinely new craft, distinct from both traditional engineering and "vibe coding."

*Part of the [Coding with AI wiki](index.md). Next: [02 · Planning & Specs](02-planning-and-specs.md)*

---

## The shift in one picture: the "AI sandwich"

The most useful frame in the corpus comes from Kieran Klaassen (GM of Cora, Every's AI email product) and Dan Shipper, with the term coined by Trevin Chow: **humans are the bread, AI is the filling.**

- **The top slice (framing / brainstorming)** is human. You decide what the real problem is, supply the constraints and taste, and notice when a plausible-looking plan is solving the wrong problem.
- **The filling (planning → implementation → review)** is increasingly automatable. "The middle is kind of solved and can be automated pretty well." — Kieran Klaassen, *The AI Sandwich* (2026-04-22)
- **The bottom slice (polish / elevation)** is human again — clicking around for *feel*, asking "what's still missing, how can it be more beautiful," because "the bar will always get higher."

Crucially, the skill is **knowing when to be in the loop**: "It's very important to know when to be in the loop versus when to hand it off, because that means we can think harder at the moments where we need to think harder." Kieran explicitly disagrees with always-human-in-the-loop dogma — the point is selective attention, not constant supervision. (This connects directly to [04 · Delegation & the Leash](04-delegation-and-the-leash.md).)

## Three kinds of engineer now exist

Dan Shipper and Brandon (Every) draw a line that recurs across the corpus (*Four Predictions for How AI Will Change Software in 2026*, 2025-12-31):

1. **Traditional engineers who bolt AI on.** They still write and read code; AI is an accelerant. "There's going to be a home and a job for you for a very long time" — but on a "slow and steady decline" (the newspaper-industry analogy).
2. **Vibe coders.** No engineering background, prompting their way to a working thing. Powerful for prototypes and personal software, but "very amateur-ish" for anything that must run in production (see [13 · Beginners & Democratization](13-beginners-and-democratization.md)).
3. **Compound / agentic engineers.** "A third thing." They have "reinvented software engineering as a skill for an agent world… not ever writing any code… fully committed to delegating all of the actual programming work… moving up a level to manage a single or multiple agents."

The corpus is emphatic that the third path is *not* vibe coding, and that **its best practitioners are excellent traditional engineers** who chose to delegate execution: "the best engineers… are also very, very good traditional engineers, just adopted this new way of building." The foundation is what lets you direct and review agents well. (This sets up a real risk — see the *judgment-development gap* in [11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md).)

## "100% thought of by me"

The slogan for the new division of labor comes from Kieran, who wrote Cora's email product with AI typing the vast majority of the code:

> "Everything is written by AI, or maybe 80% or 90%, but 100% has been thought of by me… AI helps me, but it's really a collaborator, just enabling me to do things faster." — Kieran Klaassen, *He Built an AI App With 10,000 Signups* (2025-01-15)

Guillermo Rauch (CEO of Vercel, creator of Next.js) takes the identity shift further. After coding since age 10, he no longer calls himself a coder:

> "I don't think I would identify today even as a coder… the trend has been away from the implementation detail, which is the code, and towards the end goal, which is to deliver a great product." — Guillermo Rauch, *Vercel's Guillermo Rauch on AI and the Future of Coding* (2025-02-05)

A related observation reframes what engineering even *is*: most of the job was never typing. "Maybe 20% [is coding], maybe 80% of the work is figuring out what to do next or understanding what people's feedback is." (Kieran) — which is exactly the work agents can now share.

## You are now a manager (and a capital allocator)

The most common mental model across every batch of interviews is **management**:

- Yohei Nakajima (creator of BabyAGI): "I choose when my code needs me… it's exactly how a manager operates." He builds in five-minute bursts — fire a task, walk away, come back. (*Building AI That Builds Itself*, 2024-10-23)
- Mike Taylor (Every): "Now everyone is a manager of AIs… learning how to manage AI agents is just as hard as learning how to manage humans, and nobody's had that management training yet." (*What Happens When Beginners Start Building With Claude Code*, 2026-03-17)
- Naveen Naidu (built Monologue): he toggles between "manager mode" (fire a batch of tasks at several agents, review) and "active development mode" (drive one agent directly), by mood. (*One Developer Got Thousands of Users*, 2025-09-17)

Dan Shipper packages this as the **"allocation economy"**: compensation shifts from *what you know* to *how well you allocate intelligence*. Rauch sharpens it into a capital-allocation discipline — every agent run is a bet with a real compute cost:

> "Am I going to set this entire 500-billion-[dollar] data center on fire to try to solve this task and actually don't know if the AI will perform? You have to think like a capital allocator and you have to think like a manager of these agents." — Guillermo Rauch

The skill that survives, in this frame, is the manager's judgment: "Both options [micromanaging and fully checking out] are bad, and you have to be able to know when it's important to be in the details and when it's important to just delegate — and it's very hard to know that if you're not technical." (Rauch)

## "Building is the easy part now"

Mike Krieger (co-founder of Instagram, now at Anthropic) names the consequence: when building is cheap, the scarce thing is product judgment, not implementation.

> "The models today are good at adding features. They're not necessarily good about figuring out what to cut out of the product." — Mike Krieger, *Building Is the Easy Part Now* (2026-03-25)

He had Claude rebuild Bourbon (Instagram's predecessor) in about two hours, feature-complete. The bottleneck moved entirely off of coding and onto deciding what should exist — which is why this doc hands off to [12 · Building Products When Code Is Cheap](12-building-products-when-code-is-cheap.md).

## The rote-to-art spectrum keeps moving

The corpus frames all work on a spectrum from rote to art. AI eats the rote end first — including the rote *sub-tasks* inside creative work — and pushes humans toward the creative end. The frame itself keeps moving as yesterday's craft becomes today's rote. Kieran's music analogy: a tool can generate a song, but "composing from nothing" (the start) and "performing it to people" (the end) stay human; "practicing a piece a hundred times is just work."

## Tensions & open questions

- **Do you still read the code?** Sharp, unresolved disagreement. Kieran and Geoffrey Litt happily *don't* read generated code on throwaway work ("my goal is to not understand this, my goal is to just build a thing"); Mike Taylor and Mike Krieger read and interrogate it ("you wrote the code, I don't trust you"). Both camps agree this only works if *you could* read it — see [05 · Verification](05-verification-review-and-testing.md) and [11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md).
- **Is "no more code" real or rhetoric?** "Compound engineers… not ever writing any code" is an aspiration most speakers qualify in practice (they still review diffs, debug hard bugs, and lean on deep stack knowledge).
- **The further an agent gets from a human, the less valuable it is** (Dan Shipper). The orchestration paradigm has a gravity well: value still flows through a human's framing and judgment.

## How to apply it

- Treat each task as: **(1) frame it yourself, (2) let the agent plan and build, (3) put your taste on the output.** Spend your scarce attention on the bread, not the filling.
- Decide *consciously* whether a given task is "in the details" work or "delegate and review" work. The decision is the skill.
- If you came up as an engineer, your foundation is an asset, not baggage — it's what lets you direct and review. Don't mistake "I no longer type much code" for "my judgment no longer matters."
- If you're non-technical, expect to learn *management*, not syntax — and expect it to be genuinely hard.

## See also

- [02 · Planning & Specs](02-planning-and-specs.md) — the "top slice" of the sandwich, in detail
- [04 · Delegation & the Leash](04-delegation-and-the-leash.md) — when to be in the loop vs. hand off
- [05 · Verification, Review & Testing](05-verification-review-and-testing.md) — the "bottom slice," and the new bottleneck
- [11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md) — what the compound engineer's skill set is
- [00 · Synthesis](00-synthesis-coding-with-ai.md) — how all of this fits together

## Sources

Primary: *The AI Sandwich* (Kieran Klaassen / Dan Shipper, 2026-04-22); *Four Predictions for How AI Will Change Software in 2026* (Dan Shipper / Brandon, 2025-12-31); *He Built an AI App With 10,000 Signups* (Kieran Klaassen, 2025-01-15); *Vercel's Guillermo Rauch on AI and the Future of Coding* (2025-02-05); *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *Building AI That Builds Itself* (Yohei Nakajima, 2024-10-23); *What Happens When Beginners Start Building With Claude Code* (Mike Taylor, 2026-03-17); *One Developer Got Thousands of Users* (Naveen Naidu, 2025-09-17).

*Caveat: nearly every speaker here is building or selling something in the space, and most productivity claims are single-team self-reports crystallized by one recurring host (Dan Shipper). See the [index](index.md) note on evidence.*
