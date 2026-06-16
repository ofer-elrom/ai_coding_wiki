# 02 · Planning & Specs: The Thinking Happens Before the Code

> The single most repeated practical lesson in the corpus: **review the plan while it is still text, before the agent writes a line of code.** A wrong plan is cheap to fix; wrong code is expensive. Most of the leverage in agentic coding is front-loaded into the spec.

*Part of the [Coding with AI wiki](index.md). Prev: [01 · From Writing Code to Directing Agents](01-from-writing-code-to-directing-agents.md) · Next: [03 · Context Engineering](03-context-engineering.md)*

---

## Plan mode is the highest-leverage, most-skipped move

Boris Cherny, who created Claude Code at Anthropic, gives the cleanest claim:

> "Plan mode… can two-to-three-x success rates pretty easily if you land on the plan first." — Boris Cherny, *Inside Claude Code From the Engineers Who Built It* (2025-10-29)

He also notes the boundary keeps moving: "stuff that used to be scaffolding, with a more advanced model it gets pushed into the model itself." But as of these recordings, skipping the plan is the most common beginner mistake, because the agent will proceed on a *plausible-looking but subtly wrong* framing and only reveal the error after a large implementation.

## Fix problems at the lowest-value stage

The Cora team (Kieran Klaassen and the engineer "Natesh") ground plan-first review in Andy Grove's *High Output Management*:

> "Fix any problem at the lowest value stage… check the AI's work at the lowest value stage; you want to catch those problems early." — Natesh, *How Two Engineers Ship Like a Team of 15 With Claude Code* (2025-06-11)

Dan Shipper's metaphor: "If you point a rocket at the moon, one inch [of error at launch] means thousands of miles of difference." Reviewing a plan costs minutes; reverting and re-implementing wrong code costs hours. This is the economic core of the whole approach, and it is why verification belongs at the spec stage, not only at the end — see [05 · Verification](05-verification-review-and-testing.md).

A practical refinement from the Compound Engineering plugin (*Compound Engineering: Your Questions Answered Live*, 2026-02-24): comprehensive plans tend to **over-include** (test suites, backwards-compat layers, risk mitigations you don't need for this feature). Plan review is where you *scope down*, not just sanity-check.

## Make the plan readable so you actually review it

PRDs are boring, so people rubber-stamp them. The Cora team reshapes the plan into something a human will genuinely engage with — "user stories, questions a good PM would ask, two options each" — "because it's more fun to read that than 'week one we'll do this, week two we'll do that.'" The goal is to surface red flags, not to produce a document.

Geoffrey Litt (Ink & Switch) frames the plan as a spec you read *instead of* the code: "It's writing a spec document as a product manager that we get to look at." For small throwaway apps he reviews the plan and never reads the code at all. (*You Can Build an App in 60 Minutes with ChatGPT*, 2024-01-10)

## "Muse, not oracle": bring the agent in early and have it ask *you* questions

Litt's principle: "ChatGPT is a muse, not an oracle… the moment for chat to get involved is not when the idea is fully formed — the earlier you bring it in, the more it can contribute." His single most important prompt instruction:

> "Ask the user for clarification on parts of their idea that are under-specified… once ambiguities are resolved, make a plan for how the code's going to work first, then write the code." — Geoffrey Litt

This matters because **models are good at asking clarifying questions but bad at knowing when to ask versus assume.** Boris Cherny gives the same advice from the tool-builder's side: "Claude doesn't naturally like to ask questions… I'll just tell Claude Code 'please ask me questions if there's anything you're unsure about,' and that actually helps you arrive at a better answer." Add it to your prompt explicitly.

## Voice → PRD: think out loud, then let the model structure it

A recurring workflow is dictating a stream-of-consciousness and letting the model turn it into a spec:

- Kieran builds the prompt in his head on a walk, records a 5–8 minute voice memo, transcribes it, and converts it to a PRD in the LLM — he built Cora's MVP overnight this way. Walking "enables me to just keep going… getting more into a flow state." (*10,000 Signups*, 2025-01-15)
- The critical step is **editing the PRD yourself** afterward, specifically looking for anything that misrepresents your intent. The model supplies the draft; you supply the frame. (This pattern recurs with Michael Taylor's voice→PRD→edit loop elsewhere in the corpus.)

"Context is king," and rambling for a minute gives the model far more to work with than typing a terse instruction — which is why several builders dictate rather than type (see [03 · Context Engineering](03-context-engineering.md)).

## Scope plans to the size of the task — and prefer several medium plans to one giant one

From the Compound Engineering plugin, three plan fidelities exist (minimal / more / comprehensive), and:

> "The 'a lot' plan is still the hardest to implement. The medium one is ideal. If you have multiple medium plans, it will do better than a gigantic plan." — Kieran Klaassen

For large or ambiguous projects, **brainstorm before planning**: brainstorm expands scope and surfaces ideas; planning narrows and concretizes. A multi-phase roadmap can come out of the brainstorm, with each phase executed as its own medium plan. Litt's version: "humans need to plan the right amount for the magnitude of the task" — over-planning a tiny app is as wrong as under-planning a big one.

## When to skip the plan (and the "throwaway" alternative)

The corpus is explicit that plan review is **risk-dependent**:

- **Professional / production work:** always review the plan. "In a professional setting I would always review the plan because it saves you time and also you can learn." (Kieran)
- **Throwaway / exploratory work:** it's fine to run end-to-end without reading the plan (the `/LFG` command does idea → plan → build → review → PR).

When you genuinely don't know what you want, use **throwaway development** instead of planning in the abstract: have the agent implement a one-line idea blind, watch it fail, discard it, and use what you learned to write the real plan. "Before, it'd be too expensive to yolo-send an engineer on a feature you hadn't spec'd out. But because Claude is going through your codebase… you can learn stuff from it." (Dan Shipper / Boris Cherny). It's the cheap-failure route to a good spec.

## The plan/spec doc is becoming the unit of agentic work

Several builders converge on the idea that the durable artifact is the **plan document**, round-tripped between humans and agents:

> "Create such a good plan… and then say go do that plan." — Kieran Klaassen, on the killer use case for Proof, Every's human+AI collaborative editor (*How We Use Proof*, 2026-03-11)

In that workflow, one person's agent writes a plan, shares the link, the *other* person's agent reviews and comments, the owner approves, and only then does execution begin. Plans are "really short… they help you do long-horizon things, but then they don't matter anymore" — so the spec is a transient coordination object, not permanent documentation.

## Tensions & open questions

- **When is a plan "good enough" to approve?** The corpus universally recommends plan review and universally fails to give a rubric for it. This is the single biggest gap in the whole approach — addressed head-on in [05 · Verification](05-verification-review-and-testing.md), where the scattered pieces of an approval rubric are assembled.
- **Plan-first vs. abundance.** Codex's "fire many tasks without over-specifying" philosophy (see [04 · Delegation](04-delegation-and-the-leash.md)) sits in tension with plan-first discipline. The reconciling variable: plan-first suits agents that execute live in your environment (a wrong run is costly to revert); abundance suits delegation agents that return reviewable diffs (a wrong run is just a diff you don't merge).
- **Does fast inference kill planning?** With ultra-fast models (Spark), some builders speculate a return to a single "mega-prompt" over elaborate planning — see [09 · Choosing Models](09-choosing-models-for-coding.md).

## How to apply it

- Default to **plan mode** for anything that matters; read the plan before you let it build.
- Put "**ask me clarifying questions before you start**" in your prompt. It is nearly free and consistently improves results.
- **Dictate** your intent when you can; edit the resulting PRD for anything that misrepresents you.
- Break big work into **several medium plans**, not one giant one; brainstorm first if the problem is fuzzy.
- For genuine unknowns, **throwaway-build to learn the spec**, then plan the real thing.
- Skip the plan only for low-stakes / throwaway work — and know which bucket you're in.

## See also

- [03 · Context Engineering](03-context-engineering.md) — supplying the agent the right information to plan well
- [05 · Verification, Review & Testing](05-verification-review-and-testing.md) — the missing "is this plan good enough?" rubric
- [06 · Compounding Engineering](06-compounding-engineering.md) — turning your planning ritual into a reusable command
- [04 · Delegation & the Leash](04-delegation-and-the-leash.md) — plan-first vs. abundance-mindset delegation

## Sources

Primary: *Inside Claude Code From the Engineers Who Built It* (Boris Cherny / Cat Wu, 2025-10-29); *How Two Engineers Ship Like a Team of 15 With Claude Code* (Kieran Klaassen / Natesh, 2025-06-11); *Compound Engineering: Your Questions Answered Live* (Kieran Klaassen, 2026-02-24); *You Can Build an App in 60 Minutes with ChatGPT* (Geoffrey Litt, 2024-01-10); *He Built an AI App With 10,000 Signups* (Kieran Klaassen, 2025-01-15); *How We Use Proof* (Dan Shipper / Kieran Klaassen, 2026-03-11).
