# 05 · Verification, Review & Testing: The New Bottleneck

> Generation is largely solved; review is not. Across the corpus the same diagnosis recurs: there is "too much code to review," and the human has become the bottleneck. This is the most important — and most unsolved — area of coding with AI. This doc collects what works, assembles the approval rubric the corpus keeps gesturing at, and names the tension that caps the whole productivity thesis.

*Part of the [Coding with AI wiki](index.md). Prev: [04 · Delegation & the Leash](04-delegation-and-the-leash.md) · Next: [06 · Compounding Engineering](06-compounding-engineering.md)*

---

## The bottleneck has moved from generation to verification

> "The bar was: can it get to the human without any errors? And that's basically solved… But the next bar is: does it actually work *well*? And that's still an open question." — Dan Shipper, *How Andrew Wilkinson Uses Opus 4.5* (2026-01-21)

The Codex team puts it operationally — members "complaining there is too much code to review" — and Matt Colyer (Figma) generalizes it: "We become the bottleneck then… we only have so many human eyes to review all of this work… how do we review better? That's where the bottleneck is now." (*The SaaS Apocalypse Is a Goldmine*, 2026-06-03). The Codex 2026 team notes review has even *doubled*: you review the agent's code, then review your peers' agent-produced code — "two runs of reviews."

The rest of this doc is the corpus's toolkit for making review keep pace.

## 1. Catch errors at the plan stage (the cheapest fix)

The highest-leverage verification happens before any code exists: review the plan/spec, not the finished implementation. "Fix any problem at the lowest value stage." This is covered fully in [02 · Planning & Specs](02-planning-and-specs.md); it belongs here because **plan review is verification** — the cheapest, most reversible kind.

## 2. Smoke tests so the agent self-corrects without you

The point of tests in agentic coding isn't coverage — it's giving the agent a signal to iterate against so you don't have to watch.

> A prompt failing "four out of 10 times" was handed to Claude Code to iterate: "keep going and change the prompt until it's passing consistently… I just walked downstairs, got a coffee, walked up, and that was it" — now 10/10. — Kieran Klaassen, *Team of 15* (2025-06-11)

Kieran calls smoke tests "the bare minimum": enough for the agent to know when something broke and fix it. **Treat prompt evals like tests for code** — run the prompt N times, measure the failure rate, iterate until stable. An unstable prompt is a bug, not bad luck.

## 3. Behavioral verification: prove it works, don't just read the diff

When agents generate at volume, reading every diff is infeasible. The alternative is **behavioral evidence** — screenshots, click-paths, videos — answering "does it work?" rather than "does the code look right?"

- **Cited test logs.** Codex reports both passes *and* failures with deterministic citations: "It's not just saying 'the test passed, believe me' — it's saying 'the test passed, here's a deterministic citation'" you can click to the actual log line. (Embiricos)
- **End-to-end evidence before merge.** "My first question is: is there evidence that you've actually tested this end to end — not just unit tests?" The Codex app can run itself, click through the feature, screenshot the exact path, and attach it to the PR. (Dan Shipper / Tibo Sottiaux)
- **"Proof of work / proof of use / proof of thoughtfulness."** Mike Krieger ends prompts with: "Before you PR, prove to yourself and then to me that it works as intended… you wrote the code, I don't trust you." (*Building Is the Easy Part Now*, 2026-03-25)
- **A `review-video` / Loom of it working,** and Figma's "Get Design Context" flow that attaches a screenshot to the PR so the reviewer sees the *result*, not just the code.

Tibo's prediction: "maybe that's the turning point — code review becomes less important when you can verify [behaviorally] instead."

## 4. Use AI to review AI — but know the limits

> "You can use more AI to solve the problems that AI causes… 'an intern on my team just wrote this code, can you review it?' When I run that, I trust the code more than if it was just me reviewing it." — Mike Taylor (*Beginners With Claude Code*, 2026-03-17)

Mike runs ~3 agents with the Compound Engineering plugin, which deploys ~12 **reviewer sub-agents** (a security expert, a language-best-practices reviewer, a "simplicity reviewer" that exists specifically because Claude over-produces). A **cross-model** variant is also common: have one model build and another check — "Opus builds the financial-story P&L, then Codex 5.3 checks the numbers," which caught a real Opus error (Feb-to-Feb vs. full-year). (See [09 · Choosing Models](09-choosing-models-for-coding.md).)

Caution: USV and Linear both warn against fully delegating judgment to reviewer agents, and the corpus documents no failure modes for the practice yet — "we're working on it." Trust it on testable output; keep a human on judgmental output.

## 5. Design analytical prompts to resist confirmation bias

AI mirrors the framing you give it. Andrew Wilkinson asked an AI to audit a company "as if I was suspicious," and it hallucinated mass fraud — flagging Starbucks gift-card purchases (employees buying coffee) as embezzlement:

> "I prompted it the wrong way… You gotta be really careful with this stuff." — Andrew Wilkinson

The antidote: keep analytical prompts **neutral**, set a **high flagging threshold** ("only flag if it reaches a critical point"), and treat AI output as *confirming* a suspicion you already hold, not as independent evidence for a conclusion you haven't reached. (This is one face of the broader "model as a mirror" failure mode.)

## 6. Measure quality, not volume

Karri Saarinen (CEO, Linear) attacks the natural metrics as vanity:

> "The biggest vanity metric is how much of your code is agent-written or how many PRs you're merging… you need some kind of counterbalance: what is actually the quality of this work." — *How Linear Turned AI Agents Into First-class Users* (2026-04-01)

He notes the incentive problem: "we have large companies that are token sellers… your business model is to spend more tokens." Linear's counter is a **zero-bug policy with a one-week SLA**, with coding agents doing the first-pass fix and tagging an engineer — "why do you even have bugs in your product? There's no excuse for it anymore." Track bug rate, user love, revenue — not tokens or PR count.

## 7. Silent failure is usually a context gap

The scariest failure is the confident "everything looks fine" on something broken. Kieran's example: without a hint, Claude reported a PMF survey form was working — it had silently stopped sending for 14 days; *with* the hint "look at the git history around that date," it found the exact removed line. The lesson for verification tasks: **name where you think the evidence lives.** Don't assume the agent will find the right frame on its own. (More in [03 · Context Engineering](03-context-engineering.md) and [14 · Pitfalls](14-pitfalls-limits-and-risks.md).)

---

## Assembling the missing approval rubric

Six different practical threads in the corpus all stop at "and then a human reviews it" without ever saying *what the reviewer checks for* or *what threshold makes something safe to approve.* That gap is load-bearing: the whole productivity thesis assumes review is fast and reliable, and an undefined review step is neither. The pieces to build the rubric are scattered across the corpus; assembled, they are:

1. **Pick the review mode by output type.** If success is objectively testable (code runs, form sends, query returns the right shape), require **behavioral evidence** (screenshots, pass rate over N runs) and reserve human reading for exceptions. If success is judgmental (a customer email, a strategy memo, prose in your voice), **human sign-off is non-negotiable.** This single split decides *whether you read at all.*
2. **For judgmental cases, replace "is this good?" with binary, single-dimension questions.** "Is this concise? Y/N." "Does it open with a surprising claim? Y/N." Fifteen-to-twenty binary judges aggregate far more stably than one 1–10 score (next section).
3. **Generate those criteria by diffing good vs. bad examples** — "what's the difference between these two?" — because you can recognize your standards on contact even when you can't enumerate them.
4. **Set an explicit threshold and write it down** (e.g., "approve only if all blocking criteria pass and ≤2 non-blocking flags"), calibrated to the cost of a wrong approval.
5. **Compound the misses.** Every time a bad approval slips through, add the criterion that would have caught it (the compounding move from [06](06-compounding-engineering.md), pointed at the approve/reject decision instead of at style).

The *thresholds* are domain-specific and you must set them yourself — but the *structure* of the decision is general and transferable, and building it is what turns "a human reviews it" from a rubber stamp into a real safeguard.

## Evals when there is no ground truth

For tasks with no objectively correct answer (summarizing, drafting, tone, quality), model-graded numeric evals fail predictably — the **"A-minus attractor":** every output scores ~8.5/10 regardless of quality. The corpus's converged fixes:

- **Binary criteria over numeric scales.** LLMs are bad at calibrating a continuous scale, fine at yes/no on a well-defined question; 20 binary signals aggregate stably.
- **Diff good vs. bad to generate criteria** (as above).
- **Back-test first.** Run a new prompt against historical data; "everything changed" or "nothing changed" is more valuable early than any score.
- **DSPy to optimize a taste judge.** One implementation took a Claude-written taste judge from **58.8% → 76.5%** accuracy over 85 optimization iterations; the optimized prompt was *simpler* than a hand-written one. One judge captures one dimension — combine several single-dimension judges.
- **Match eval depth to product maturity:** "vibe checks" early, labeled datasets at productionization, a logging/labeling loop after deploy. "You need to evaluate eval." And upgrading the *judge model* sometimes fixes a failing eval before any redesign (Opus 4 was reportedly the first model to give consistent boolean judgments with rationale instead of rubber-stamping).

This is its own discipline; the above is the operational core.

## The tension that caps the whole thesis

Two corpus claims, each strongly held, collide:

- **Push autonomy hard** — parallel agents, ~60 automations, yolo mode, "15× output."
- **Verification is non-negotiable** — for judgmental output, "no AI output should be published or acted on without human review."

Human review is serial and runs at human speed. So **for judgmental work, your throughput is capped at your *review* rate, not your *generation* rate** — and the corpus's own evidence says the output types that *require* human review are exactly the high-value, judgment-heavy ones that lack an objective test. Practical consequences:

- Forecast capacity from your **review** rate for judgmental work; the lever is making review faster (binary checklists, behavioral evidence, reviewer agents), not generating faster.
- Reserve long leashes and automations for **machine-checkable** tasks; violating this just moves the work onto a human reviewer who is now the bottleneck.
- **Discount cross-domain productivity claims by testability.** A 15× number from a coding team (testable) should not be transplanted onto a legal or marketing team (judgmental).

The cap softens only if reviewer sub-agents become trustworthy for judgmental output — early movement exists (Mike Taylor trusting reviewer sub-agents "more than my own code review"), but the corpus is explicit it's unsolved.

## The reproducibility problem (for regulated / enterprise buyers)

Paul Ford (Aboard) names a constraint the consumer-builder enthusiasm glosses over:

> "That lack of reproducibility is really scary because they need to know that something is the same today as it was yesterday." — *Why Opus 4.5 Just Became the Most Influential AI Model* (2026-12-03)

His proposed UI metaphor: a **GitHub-commit-style state log** ("here was our state, then I transformed it into this new state; I've saved the old state in case we want to go back") instead of an anthropomorphic chat that "looks like it's answering rather than statistically translating." Treat the LLM as a *translator* of intent into form, not as an answer engine.

## How to apply it

- Move as much verification as possible to the **plan stage**.
- Give agents **smoke tests / evals** to self-correct against; treat an unstable prompt as a bug.
- Demand **behavioral evidence** (screenshots, click-paths, cited logs) over reading diffs for high-volume work.
- Use **reviewer sub-agents and cross-model checks** for testable output; keep a human on judgmental output.
- Build and write down an **approval rubric** (mode-by-output-type → binary criteria → threshold → compound the misses).
- Keep analytical prompts **neutral** with a high flagging threshold.
- Plan capacity around your **review rate** for judgmental work, and don't transplant a coding team's multiplier onto a judgment-heavy team.

## See also

- [02 · Planning & Specs](02-planning-and-specs.md) — plan-stage review, the cheapest verification
- [04 · Delegation & the Leash](04-delegation-and-the-leash.md) — verifiability is the variable behind autonomy
- [06 · Compounding Engineering](06-compounding-engineering.md) — encoding review learnings into lint rules and judges
- [09 · Choosing Models for Coding](09-choosing-models-for-coding.md) — cross-model verification; judge-model choice
- [14 · Pitfalls, Limits & Risks](14-pitfalls-limits-and-risks.md) — silent failure, non-determinism, "app glaze"

## Sources

Primary: *How Andrew Wilkinson Uses Opus 4.5* (2026-01-21); *The SaaS Apocalypse Is a Goldmine* (Matt Colyer, 2026-06-03); *OpenAI's Codex This Model Is So Fast* (Tibo Sottiaux, 2026-02-18); *OpenAI Launches Codex* (Alexander Embiricos, 2025-05-16); *How Two Engineers Ship Like a Team of 15 With Claude Code* (Kieran Klaassen, 2025-06-11); *What Happens When Beginners Start Building With Claude Code* (Mike Taylor, 2026-03-17); *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *How Linear Turned AI Agents Into First-class Users* (Karri Saarinen, 2026-04-01); *Why Opus 4.5 Just Became the Most Influential AI Model* (Paul Ford, 2026-12-03); plus the corpus's evals material (Spiral / DSPy, the audience-simulator binary-criteria method, the "A-minus attractor").
