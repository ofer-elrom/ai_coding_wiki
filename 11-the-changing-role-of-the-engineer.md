# 11 · The Changing Role of the Engineer: Skills, Taste, Careers & Teams

> If implementation is increasingly automatable, what is the engineer's durable contribution — and how do you build it? The corpus's answer is "re-level upward to judgment, framing, and taste." Its sharpest unresolved worry is that the very work AI takes off your plate is the work that *builds* that judgment.

*Part of the [Coding with AI wiki](index.md). Prev: [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md) · Next: [12 · Building Products When Code Is Cheap](12-building-products-when-code-is-cheap.md)*

---

## Re-level upward — to the bread of the sandwich

The near-unanimous strategic advice is to move to higher-altitude work: problem framing and quality elevation, the two human ends of the [AI sandwich](01-from-writing-code-to-directing-agents.md). "Implementation — the translation of clear intent into correct output — is reliably automatable" for a growing range of tasks; what remains is *deciding what to build*, catching when the spec solves the wrong problem, and judging whether the output actually feels right.

The diagnostic test: look at your last ten meaningful pieces of work and ask, for each, whether the blocking constraint was **execution skill** or **judgment/framing/taste**. If it was execution, that work is on its way out; if it was judgment, that's where to invest.

## The meta-skills that survive

The corpus names specific durable skills:

- **Token breadth / "symbolic systems"** (Guillermo Rauch): a wide vocabulary of how technologies and concepts relate, so you can point an agent at the right approach without executing it. Analogy: a VC who knows thirty companies in a space and recognizes which is relevant. "You can point the agent to use a specific technology that might be the right solution, but you're not actually doing the coding."
- **Logical reasoning, problem decomposition, architecture** (Scott Wu): "these skills have *more* value in the AI era, not less, because humans remain at the helm making these calls." Education shifts toward "deeply understanding logical fundamentals" and away from "debugging your Kubernetes or esoteric syntax."
- **Executive function / project initiation** (Tyler Cowen): the ability to *decide what to go do* — to generate the project, not just execute it. "AI doesn't make the decision to go out and do the idea."
- **Taste — and it's trainable.** Rauch: "I just train my taste a lot. I look at a lot of things, I look at what people like, I seek a lot of feedback." Plus product thinking — understanding the end user well enough to know what to build.

These compound best **on top of genuine domain knowledge.** The compound-engineer path is "most powerful when the person has a strong traditional engineering foundation" — they just chose to delegate execution.

## "Bricklayers into architects" — and a real problem for juniors

Scott Wu's optimistic framing: AI turns "bricklayers into architects," and beginners can now build interesting things on "your first prompt" instead of grinding six months of while-loops first ("officer's school" instead of hazing). The calculator analogy: "when calculators first came out, there were a lot of protests… we did okay as a society."

But the corpus's own most important insight cuts against the easy version of this advice:

> **AI is best at automating exactly the rote micro-decisions through which judgment, taste, and product sense are *built*.** Delegating that work doesn't just save time — it removes the training reps for the one capability the corpus insists is your only defense.

Mike Krieger names the same thing from the product side: going "zero to near-complete in hours skips the incremental decision-making that builds deep product sense" — the "indoor tree" that never felt the wind. GoodStar names it as the labor-market version: "AI leverage paradox — senior benefit, junior hollowing," with apprenticeship-plus-AI offered as a *partial* answer. The practical implication: **delegate freely in domains where your judgment is already trained; keep doing enough micro-decisions by hand where it isn't** — and route some formative work to juniors *unautomated*, using AI as the tutor, not the doer.

## "There's no such thing as a technical person, just a persistent one"

Mike Taylor reframes the entry barrier as persistence and vocabulary, not capability:

> "It's a persistence gap… when I get an error now with Claude, I think 'oh great, there's an opportunity to figure out something the models aren't good at yet.'" — *Beginners With Claude Code* (2026-03-17)

This is empowering, but it coexists with the judgment-gap caution above: persistence gets you a shipping artifact; it does not, by itself, build the taste to know whether the artifact is good. (See [13 · Beginners & Democratization](13-beginners-and-democratization.md).)

## Some "old" skills are *more* valuable now

Counterintuitively, deep systems knowledge resurges. Mike Krieger:

> "I thought my skills in distributed systems were not going to be useful anymore, but actually those are maybe some of the most useful skills." — His Redis-vs-Postgres debate with Claude "only worked because I was grounded in having used a lot of these technologies before."

The anti-pattern he warns against is papering over a non-robust system with a patch ("just retry in 5 seconds") — the same shape as bolting "never, all caps, use markdown" onto a prompt. Senior judgment is what catches the "tower of assumptions you're not fully aware of."

## "Slap work" and why experts get *more* valuable

When AI makes expert-looking output cheap, the value of surface competence falls and the value of genuine judgment rises — because someone has to spot the difference. Every calls the cheap-but-wrong output **"slap work":** "close but not quite right" for the specific situation. The result is *more* demand for experts, not less — to build the oversight systems, set review standards, and catch what AI misses. (*We Automated Everything With AI and Tripled Our Headcount*, 2026-05-27)

## Team shape: small, and don't pre-scale

A two-person team "that feels like 15" is the headline (Cora), but the deeper lesson is about *not* growing:

> "Scaling the teams too quickly is a net negative… they end up spending all this time on coordination." — Mike Krieger

While an idea still fits in one head, adding people hurts. The threshold "where you can't hold the entire thing in your head used to be much smaller; now it's much bigger." The gating factor for a project is **conviction** — bets that died, in postmortem, had nobody who thought it was "the thing." (Anthropic Labs has "only one PM for all of Labs" and re-evaluates every project every two weeks.) And headcount can go *up* even with heavy automation — Every grew from ~4 to ~30 while being maximally AI-native, because AI "increases the demand for experts" to finish the flood of close-but-not-quite output.

## Hiring for the AI era

- **Hire for the aesthetic they already ship.** "If the aesthetic you already see them do is exactly what you want, that's a great sign. If you want them to do something totally different than they've ever done, that's harder." (Monologue found its iOS engineer off the button shadows in his prior app.)
- **Lightly-technical GMs who spike on taste** can now run products, but you still need **senior systems people** even in a 0→1 lab (Krieger). Designers increasingly "split designer and builder," writing "almost as much code as the engineers."
- **Hunger over entrenched experience.** GoodStar hires "hungry young people without entrenched habits over experienced people whose experience may now be wrong — because the whole landscape just changed." The EIR test at Every: does a call shift "from talking about their background to just riffing on an idea"? (technical + taste + EQ).

## What stays human (the deepest cut)

Dan Shipper's formulation of the durable edge:

> "Once it's articulated, once it's specified, a model can climb on it and beat it… there's actually a lot of stuff that you do that can't be articulated in a clean frame." — *We Automated Everything* (2026-05-27)

So the human contribution concentrates in the **unarticulable** and in **deciding what matters**. Paul Ford locates it in the "infinite library of meaningful stories" regime — domains with no single right answer (strategy, design, narrative), where a confident model output should not be mistaken for *the* answer the way a spreadsheet computation can be.

## Tensions & open questions

- **"No programmers in 2–3 years" vs. "judgment survives."** Andrew Wilkinson (echoing Amodei) vs. Paul Ford / Dan Shipper. Both agree the *role* transforms; they disagree on whether "programmer" as a category persists.
- **The junior pathway is genuinely unresolved.** Apprenticeship-plus-AI is the only proposed fix for a problem the corpus elsewhere proves is real.
- **Honor the grief.** A minority but serious dissent (Joe Hudson): identity collapse from skill commoditization is an emotional problem before it's a strategy problem — "if grief doesn't come, your transformation ain't coming either." Sequencing matters: process first, re-level second.
- **Does AI-mediated practice build judgment as well as unassisted practice?** If AI-as-tutor can re-create the formative reps, the judgment gap dissolves into a tooling question. Untested.

## How to apply it

- Audit your work into **execution vs. judgment**; invest where the constraint is judgment.
- Build **meta-skills** (token breadth, decomposition, architecture, taste, product sense) on top of real domain knowledge.
- In unfamiliar domains, **keep doing the micro-decisions by hand** to build a model; use AI as a tutor.
- Protect **junior development paths** — route some formative work unautomated.
- Keep teams **small** until an idea outgrows one head; hire for **demonstrated aesthetic** and **hunger**.
- Concentrate your edge on the **unarticulable** and on **deciding what matters**.

## See also

- [01 · From Writing Code to Directing Agents](01-from-writing-code-to-directing-agents.md) — the three kinds of engineer; the AI sandwich
- [13 · Beginners & Democratization](13-beginners-and-democratization.md) — the entry barrier and where it really sits
- [12 · Building Products When Code Is Cheap](12-building-products-when-code-is-cheap.md) — where the scarce value migrates
- [05 · Verification, Review & Testing](05-verification-review-and-testing.md) — judgment as the human review layer

## Sources

Primary: *Vercel's Guillermo Rauch on the Future of Coding* (2025-02-05); *Cognition's CEO on What Comes After Code* (Scott Wu, 2025-09-24); *Economist Tyler Cowen on How ChatGPT Is Changing Your Job* (2024-01-24); *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *What Happens When Beginners Start Building With Claude Code* (Mike Taylor, 2026-03-17); *We Automated Everything With AI and Tripled Our Headcount* (Dan Shipper / Brandon, 2026-05-27); *Four Predictions for How AI Will Change Software in 2026* (2025-12-31); *Meet the Team Behind Monologue* (2026-02-19); *How Andrew Wilkinson Uses Opus 4.5* (2026-01-21); *Why Opus 4.5 Just Became the Most Influential AI Model* (Paul Ford, 2026-12-03).
