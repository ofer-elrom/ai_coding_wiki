# Coding With AI: A Field Guide From Every's "AI & I"

*The synthesizing essay for this wiki. Start at the [index](index.md) for the full map; this piece ties the fourteen subject documents into one argument.*

---

## The one-sentence version

Across roughly forty conversations — with the engineers who built Claude Code and Codex, the CEOs of Cognition, GitHub, Linear, Vercel and Figma, and a stream of small teams shipping real products — the same story keeps surfacing: **the cost of writing software has collapsed, so the job has moved up a level — from writing the implementation to framing the problem, directing agents, and judging the result.** "Building is the easy part now." Everything else in this wiki is a consequence of that sentence.

## The spine: the AI sandwich and the manager

The most useful single frame is Kieran Klaassen's **"AI sandwich"**: humans are the bread, AI is the filling. You own the top slice (framing the problem, supplying constraints and taste) and the bottom slice (polish, the judgment of whether it's actually good). The middle — plan, implement, review — is increasingly automatable. The skill is *knowing when to be in the loop* so you can "think harder at the moments where we need to think harder." ([01 · From Writing Code to Directing Agents](01-from-writing-code-to-directing-agents.md).)

The second frame is **management**. "Now everyone is a manager of AIs," and managing them is "just as hard as learning to manage humans." Dan Shipper's sharper version — the **allocation economy** — says compensation shifts from *what you know* to *how well you allocate intelligence*, and Guillermo Rauch turns it into capital allocation: every agent run is a bet with a real compute cost. The recurring punchline, from someone who runs a company on this: "If you're not a good manager, you've never managed anybody, you're not going to be very good at using AI." ([04 · Delegation & the Leash](04-delegation-and-the-leash.md).)

A useful distinction sits underneath all of it: a **third kind of engineer** has appeared, beyond "traditional engineer who bolts AI on" and "vibe coder." The *compound engineer* delegates implementation entirely and works one level up — and the best of them are excellent traditional engineers who chose to. "It's not vibe coding… it's a third thing."

## Eight through-lines that recur in every subject

**1. The work moved up a level.** Implementation is the cheap part; framing, decomposition, architecture, and taste are the scarce part. "100% has been thought of by me," even when AI typed 90% of the code. Rauch no longer calls himself a coder. ([11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md).)

**2. Leverage concentrates in the plan.** The most-repeated practical lesson in the entire corpus: **review the plan while it's still text.** Plan mode "can two-to-three-x success rates." A wrong plan is minutes to fix; wrong code is hours. "Fix any problem at the lowest value stage." ([02 · Planning & Specs](02-planning-and-specs.md).)

**3. Context is the real substrate.** "Every problem becomes a context problem." Most agent failures are not the model being dumb — they're the model missing the right information or drowning in the wrong information ("context rot"). The craft is a lean job-description file plus situational learnings retrieved on demand, not one bloated config. ([03 · Context Engineering](03-context-engineering.md).)

**4. Verification is the new bottleneck.** Generation is largely solved; review is not. "Too much code to review." The frontier moves from reading diffs to **behavioral evidence** — screenshots, click-paths, cited test logs, "prove to yourself and then to me that it works." This is the corpus's most active and least-solved area. ([05 · Verification, Review & Testing](05-verification-review-and-testing.md).)

**5. Compounding beats one-off speedups.** "Instead of doing the work, do the thing that does the work going forward." Encode every learning back into commands, skills, hooks, lint rules, and tests so the tenth task is cheaper than the first. Anthropic writes ~100% of its tests and 100% of its internal lint rules this way. ([06 · Compounding Engineering](06-compounding-engineering.md).)

**6. Models keep moving — so build for the curve and ride them.** Capability improves on a known trajectory and gets ~10× cheaper every few months. So: build scaffolding you'll happily delete, "don't solve problems the next model one-shots," keep an eval suite to migrate the day a new model ships, and "throw out your old stuff every three months." Pick models by *personality and task*, not just benchmark. ([09 · Choosing Models](09-choosing-models-for-coding.md), [08 · Tools & Harnesses](08-tools-harnesses-and-form-factors.md).)

**7. Value migrates to what code no longer protects.** When anyone can build it, the moat isn't the build. It's taste/conviction, relationship capital, deep vertical expertise, business model, brand, and distribution. The first discipline of product is now *what to cut*, not what to add. ([12 · Building Products When Code Is Cheap](12-building-products-when-code-is-cheap.md).)

**8. Agents become users, so the software around them must change.** Interfaces, identity, auth, payments, and even how you expose an API all have to be rebuilt for non-human actors. "Machines are becoming users of developer infrastructure" — LLM traffic to Stripe's docs is up 10× year over year. ([10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md).)

## The genuine disagreements (don't let anyone flatten them)

A faithful reading of the corpus surfaces real, unresolved tensions. They're where the interesting decisions live:

- **Do you read the generated code?** Kieran and Geoffrey Litt proudly don't on throwaway work ("my goal is to not understand this"); Mike Taylor and Mike Krieger interrogate every diff ("you wrote the code, I don't trust you"). Both camps agree it only works if you *could* read it.
- **Plan-first vs. abundance.** Plan-and-review discipline (Cora) vs. "fire many tasks without over-specifying" (Codex). Reconciled by the cost of a wrong run: live-in-your-environment agents need plans; diff-returning agents can be fired in bulk.
- **Generic harness vs. harness+model pairing.** Anthropic says the harness and model are becoming tightly paired; Stainless and Linear want model-agnostic surfaces. A real bet with real consequences.
- **Leash length / verifiability.** "Short leash for taste, long leash for functional" is a proxy; the real variable is **whether you can check the result without having been present** — which dissolves the hybrid-task problem if you decompose by checkability.
- **Is prompt engineering dead?** The "$10,000 tip" trick is dying; richly specifying what you want is not, and is "really, really important."
- **"No programmers in 2–3 years" vs. "judgment survives."** Wilkinson (echoing Amodei) vs. Ford and Shipper. Everyone agrees the role transforms; they disagree on whether the category persists.
- **Speed inverts the playbook.** Fast models pull work *back* toward synchronous flow, make tool calls the slow part, and can make sub-agents net-negative — reviving the "mega-prompt." The async-and-orchestrate gospel is not monotonic.
- **Democratization: real but bounded.** Genuinely open for personal/throwaway software; production still rewards judgment, and the success stories often hide a programming head start.
- **SaaS apocalypse vs. expansion.** "Code is free, margins compress" (Wilkinson) vs. "more developers means more software means a bigger tooling market" (Colyer).

## The three worries the corpus can't yet answer

Beneath the optimism sit three problems the practitioners themselves keep circling without resolving:

1. **The judgment-development gap.** The corpus says your durable edge is judgment and taste — and says to delegate the rote work. But the rote work is the stream of small decisions through which judgment is *built*. Follow the productivity advice literally and you can hollow out the very asset you're told to protect. It bites hardest on juniors ("senior benefit, junior hollowing") and on anyone working in an unfamiliar domain. The only proposed fix — apprenticeship plus AI-as-tutor — is partial. ([11](11-the-changing-role-of-the-engineer.md).)

2. **The missing approval rubric.** Nearly every workflow ends at "and then a human reviews it," and none define what *good enough to approve* means. The whole productivity thesis assumes that review step is fast and reliable; undefined, it's neither. The pieces of a rubric exist (mode-by-output-type → binary criteria → explicit threshold → compound the misses); assembling it is the single highest-leverage thing a serious team can do. ([05](05-verification-review-and-testing.md).)

3. **Verification caps the multiplier.** "Generation is solved, push autonomy" and "for judgmental output, human review is mandatory" collide: human review runs at human speed. So the headline multipliers are real where success is machine-checkable and shrink — possibly to nothing — where it isn't. Plan capacity, pricing, and headcount around your *review* rate, not your generation rate, for judgment-heavy work. ([05](05-verification-review-and-testing.md), [14 · Pitfalls](14-pitfalls-limits-and-risks.md).)

## How much to believe

This corpus is unusually candid, and it deserves a candid reading. Almost every claim here is **single-team, self-reported anecdote or live demo**, not a controlled study. The product being demoed is frequently also the thing being sold. One recurring host — Dan Shipper, himself a builder and author in this space — shapes much of the framing, so apparent "consensus" is sometimes one worldview repeated by friendly guests. And the most quotable numbers ("15× output," "$100k of engineers for $40/day," "tripled headcount") are advertising until you've measured them yourself.

That is not a reason to dismiss it. The convergence across genuinely independent practitioners — Anthropic and OpenAI and Cognition and Vercel and Linear and a dozen small teams arriving at the same workflow primitives — is real signal. The right posture is: **treat the direction as well-supported and the magnitude as unproven.** Where the corpus contradicts itself, the contradiction is usually the most useful part. ([14 · Pitfalls, Limits & Risks](14-pitfalls-limits-and-risks.md) collects the failure modes in full.)

## Where to start

- **If you're an engineer adopting agents:** [02 · Planning & Specs](02-planning-and-specs.md) → [05 · Verification](05-verification-review-and-testing.md) → [06 · Compounding](06-compounding-engineering.md) → [04 · Delegation](04-delegation-and-the-leash.md). These four are the day-one operating system.
- **If you're non-technical and want to build:** [13 · Beginners & Democratization](13-beginners-and-democratization.md) → [01 · Directing Agents](01-from-writing-code-to-directing-agents.md) → [02 · Planning & Specs](02-planning-and-specs.md).
- **If you're a founder or PM:** [12 · Building Products When Code Is Cheap](12-building-products-when-code-is-cheap.md) → [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md) → [11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md).
- **If you're skeptical (good):** [14 · Pitfalls, Limits & Risks](14-pitfalls-limits-and-risks.md) first, then come back to [05 · Verification](05-verification-review-and-testing.md).

## The throughline, restated

The optimistic and the cautious readings of this corpus are the same fact seen from two sides. Building got cheap, so the scarce things are now **judgment, taste, framing, verification, and distribution** — and the open question, which the corpus is honest enough to leave open, is whether we can keep *developing* those scarce human capabilities while handing the practice that builds them to the machine. The teams that win will be the ones who delegate the work **and** deliberately keep the reps that make them worth delegating to.

---

*Continue to the [index](index.md) for the full document map, reading paths, source list, and the cast of voices.*
