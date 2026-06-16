# 14 · Pitfalls, Limits & Risks

> The honest counterweight to the rest of this wiki. The same practitioners who report 15× output are unusually candid about where AI coding breaks: silent failures, context rot, bloat, security holes, non-determinism, new fraud vectors, and quality problems beginners can't yet perceive. This doc collects the failure modes — and the meta-caveat that most of the wiki's evidence is single-team self-report.

*Part of the [Coding with AI wiki](index.md). Prev: [13 · Beginners & Democratization](13-beginners-and-democratization.md)*

---

## Silent failure and confident wrongness

The most dangerous failure is the confident "everything looks fine" on something broken:

- An agent reported a PMF-survey form was working; it had silently stopped sending for **14 days**. Only with a directional hint ("look at the git history around that date") did it find the removed line. Silent failure is usually a **context gap**, not a model deficiency — see [03 · Context Engineering](03-context-engineering.md) and [05 · Verification](05-verification-review-and-testing.md).
- **Hallucinated charts and citations.** ChatGPT will render a plausible chart from a static variable of random data, with a disclaimer "nobody reads" (Dohmke). AI-generated docs include plausible-but-fake URLs (USV's cursor-hover trick: a text cursor over a link means it's fake).
- **Confirmation bias.** Prompt an audit "as if I was suspicious" and it hallucinates fraud (Wilkinson's Starbucks gift-card example). AI mirrors your framing back at you (see [05](05-verification-review-and-testing.md)).

## Context rot

More context is not better. "The more information you put into that chat session, the more likely it is to go off the rails… almost every AI failure traces back to context imbalance." (Mike Taylor). The fix is to start fresh sessions and isolate context in sub-agents — but no one offers a clean heuristic for *when* the threshold is crossed. See [03 · Context Engineering](03-context-engineering.md).

## Bloat, debt, and the maintainability problem

When shipping is trivial, products accrete:

- **"Vegetables" pile up.** "You can implement so many features that they start to crowd in on each other… the product is way too bloated and no one's stepped back." (Embiricos)
- **Products feel incohesive** because "you could just add a button here and a tab there" (Boris Cherny) — more code also means more code to *delete* and heavier review.
- **The "tower of assumptions."** Mike Krieger: agents make "reasonable-ish but not optimal" choices the human didn't sanction; without interrogation you build "a tower of assumptions you're not fully aware of." Papering over a non-robust system with a patch ("just retry in 5 seconds") is the recurring anti-pattern.
- **"I can vibe-code it in a weekend" is a trap.** Cora's v1 took 4 hours; production at thousands of users is "a hybrid of rule-based algorithms + LLMs + embeddings that all need to work in harmony," with LLM costs "on target to burn hundreds of thousands of dollars per year" and rate limits hit "every day." Running it reliably is the hard, paid-for part (see [12 · Building Products](12-building-products-when-code-is-cheap.md)).

## Over-trust: pushing code you don't understand

A self-reported failure mode from an AI-native company:

> "I have pushed so many PRs where I'm like, 'Willie, I literally have no idea if this works, but here you go.'" — Dan Shipper. Willie's response: "this is a good idea, but I just completely redid it." — *We Automated Everything* (2026-05-27)

The defense is **systems/guardrails so work is 'reasonably good' before an expert sees it** (repo rules, review guidelines) — but the line-crossing itself is a real and common cost. And one-shotting too much breaks in production: Austin "unleashed" a full Discord-bot go-live and it broke the instant a user hit an unbuilt feature ("I was cocky being like 'we could definitely one-shot this'… and then it just broke").

## Security and the autonomy aperture

Long-running, tool-wielding agents create a genuinely new security surface:

- **Cut the network.** Codex runs setup commands with internet, then **cuts internet access before the agent runs** — "for absolute maximum safety… there's a tail risk of exfiltration." (Embiricos)
- **Earn root access over time.** Mike Taylor runs `--dangerously-skip-permissions` on a couple of machines but "built up that trust over a long time… you don't give a new employee a corporate bank account on day one." Sensitive/production codebases get a developer sandbox.
- **Security belongs at the API layer** (OAuth scopes), and sandbox egress should be locked to a single host — see [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md).
- **The yolo-vs-gated boundary is the open product question.** OpenClaw "emailed my contacts and I didn't even know it could do that" (Krieger). The 2026 frontier is the shape "between a yolo product and one gated with permissions for good reasons." Move off vector embeddings also reduces a security/maintenance surface (Boris Cherny). And agents are **non-deterministic** — the same task yields different files each run, unlike a CI script (Dohmke).

## Reproducibility: "works like computer"

For regulated and enterprise buyers, non-determinism is disqualifying until handled:

> "That lack of reproducibility is really scary, because they need to know that something is the same today as it was yesterday." — Paul Ford (*Why Opus 4.5…*, 2026-12-03)

Proposed mitigation: a commit-style **state log** rather than an anthropomorphic chat, and treating the LLM as a *translator* of intent into form, not an answer engine. See [05 · Verification](05-verification-review-and-testing.md).

## "App glaze" — quality you can't yet perceive

> "You feel like you've captured the ultimate Pokémon, but everyone's getting the same Pokémon shoved into the mailbox." — Paul Ford

Vibe-coded apps may have quality problems you can't perceive yet ("app glaze"). The one mercy specific to software: it has a hard truth — "it pulls from the database or it doesn't… there's no API glaze." Unlike image or text slop, broken software eventually announces itself. But the gap between "looks done" and "is good" is exactly what beginners can't see (see [13 · Beginners & Democratization](13-beginners-and-democratization.md)).

## The new fraud surface

Stripe's data shows the agent economy created fresh abuse vectors (see [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md)): **"free compute is the new CAC,"** free-trial abuse 4×'d in six months, and one company has Stripe blocking 250,000 fraudulent free trials a week. When every API call costs real money, stealing the free tier becomes "an existential risk."

## The limits of the paradigm itself

- **"Squishy" problems resist it.** Labs optimize step-verifiable domains (code, math). "What will become apparent once that's done is how few problems in the world are actually like that." Cora's hardest problems are "very, very mushy" — taste, intuition, a do→get-data→retry loop with no clean reward. (Kieran)
- **Models stay weak at framing.** Rare, hard-won expert judgment is hard to get into a model ("a couple moments in your career where you gather the data"), and models are weak at *flipping frames* (knee hurts → Advil → stretch → stop running on hard surfaces). Output trends generic — "superintelligence in a box… kept in a box for the last year with no idea what's going on in the world."
- **Genuine autonomy/agency isn't here.** Current agents are *compliant by design*; "it's not changing frames, it's not finishing the task and picking the next one." Always-on autonomy needs "fundamental changes to the language model architecture." (See [04 · Delegation](04-delegation-and-the-leash.md).)

## The treadmill is real and tiring

Best practices flip every 3–6 months as the bottleneck moves (e.g., sub-agents help — until a fast model makes them net-negative). "To take advantage of it, you have to be willing to throw out your old stuff every three months and start over… it's fun and also it feels a little tiring." The meta-skill is re-deriving your own workflow, not cargo-culting whatever's hot. (See [09 · Choosing Models](09-choosing-models-for-coding.md).)

## Adoption is slower and more human-gated than the hype

Even inside elite early-adopter shops, adoption is gradual: Kate Lee resisted AI editing for *years* because it "just creates more work" until operational tasks clicked. Companies that fired customer-service reps "called them back two months later because customers refused to talk to machines." Full transformation "probably takes a generation." Layoffs blamed on AI are, by multiple accounts, mostly cover for pre-existing business problems ("we should expect companies that are not doing well to lay people off and then blame AI"). And big bureaucracies move slowly "for good reasons" — engineers "say no for good reason."

## The meta-caveat: weigh the evidence honestly

Most of this wiki rests on a specific kind of evidence, and it's worth naming:

- **Single-team, self-reported anecdote and live demo,** not controlled studies. "15× output," "two weeks of work in an afternoon," "$100k/month of engineers for $40/day," "tripled headcount" — these are first-person claims, not measurements.
- **The product is often both the evidence and the sales pitch.** Many speakers are demoing the very tool they sell (Anthropic on Claude Code, OpenAI on Codex, Vercel on v0, Every on its own products).
- **One recurring host (Dan Shipper) is also a builder and author** in this space, so apparent cross-source "consensus" is sometimes one worldview repeated.
- **Productivity multipliers are real where output is machine-checkable and shrink where it isn't** (the verification cap — see [05 · Verification](05-verification-review-and-testing.md)). Don't transplant a coding team's numbers onto judgment-heavy work.

None of this means the lessons are wrong — the convergence across many independent practitioners is genuinely informative. It means: treat the *direction* as well-supported and the *magnitude* as advertising until you've measured it yourself.

## How to apply it

- Assume **silent failure**: name where the evidence lives, demand behavioral proof, keep analytical prompts neutral.
- Treat **too much context** as a real risk; start fresh, isolate in sub-agents.
- Budget for **production reality** (cost, rate limits, maintenance), not the weekend-prototype illusion.
- **Earn** elevated permissions over time; cut network egress; put security at the API layer.
- Expect a **treadmill** and slow, human-gated adoption; don't over-index on demo-day magnitude.
- Read every productivity claim (including this wiki's) as **direction, not measurement.**

## See also

- [05 · Verification, Review & Testing](05-verification-review-and-testing.md) — the bottleneck, the approval rubric, the ROI cap
- [03 · Context Engineering](03-context-engineering.md) — context rot and silent failures
- [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md) — agent security, identity, the fraud surface
- [00 · Synthesis](00-synthesis-coding-with-ai.md) — how the optimism and the caveats fit together

## Sources

Primary: *We Automated Everything With AI and Tripled Our Headcount* (2026-05-27); *How We Built Our AI Email Assistant Cora* (2025-06-26); *OpenAI Launches Codex* (2025-05-16); *Inside Claude Code* (2025-10-29); *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *What Happens When Beginners Start Building With Claude Code* (Mike Taylor, 2026-03-17); *Why Opus 4.5 Just Became the Most Influential AI Model* (Paul Ford, 2026-12-03); *What the Agent Economy Looks Like From Inside Stripe* (Emily Sands, 2026-04-29); *How Every Edits in the Age of AI* (Kate Lee, 2026-03-18); *GitHub CEO on the AI Coding Arms Race* (Thomas Dohmke, 2025-05-28); *The AI Sandwich* (2026-04-22). The meta-caveat on evidence reflects patterns across the whole corpus.
