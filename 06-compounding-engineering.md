# 06 · Compounding Engineering: Build Systems That Make the Next Task Cheaper

> In traditional engineering, every feature makes the next one harder to add. The goal of *compound engineering* is to invert that: encode every learning back into the system — commands, skills, hooks, lint rules, tests — so the tenth task costs far less than the first. The slogan: **"Instead of doing the work, do the thing that does the work going forward."**

*Part of the [Coding with AI wiki](index.md). Prev: [05 · Verification, Review & Testing](05-verification-review-and-testing.md) · Next: [07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md)*

---

## The core idea

> "In normal engineering, every feature you add makes it harder to add the next one. In compounding engineering, your goal is to make the next feature easier." — Dan Shipper, summarizing the approach (*Inside Claude Code*, 2025-10-29)

The trigger to compound is the feeling **"I don't want to do this again."** The loop, from the Compound Engineering plugin, is **plan → work → review → compound**: at any moment something is worth keeping, you "call the compound flow and it learns from what you do" and codifies it into the system. (*Compound Engineering: Your Questions Answered Live*, 2026-02-24)

## A prompt that builds prompts

The signature move from the Cora team is to make **task creation itself** a reusable command rather than something you redo by hand. Their custom command takes a one-line feature description, grounds itself in the codebase, web-searches best practices and open-source patterns, presents a plan for human approval, then files a structured GitHub issue in the right lane.

> "What you did first is spend time building a prompt that effectively builds other prompts… every time you want to make a new feature, you have to specify less." — Dan Shipper (*Team of 15*, 2025-06-11)

Construction method the corpus recommends: start with a rough description, run it through an LLM to improve it (the Anthropic Console prompt improver with "thinking" on is named), then save it as a command. The rule: **the second time you write a long prompt, turn it into a command.**

## Encode review learnings *immediately*, while context is fresh

The highest-leverage moment to compound is right after a review, while the context window still holds what went wrong and why. The Cora team operationalized this:

> They recorded pair code reviews by voice — one person rambling about "when to make a method longer or shorter" — fed the transcript to an LLM, and distilled it into a reusable `key-run-review` Claude command. — *How We Built Our AI Email Assistant Cora* (2025-06-26)

The same pattern productized an editor's taste into automated style checks and copy-editing into auto-run commands. The summary: "It takes less time for each successive code review." Crucially, AI slop in a review is the *learning moment*, not just a failure: "that's the moment where we learn, because we talk about why it is a mess… to extract that and make it better next time."

## Turn corrections into lint rules and tests, not one-off comments

Anthropic's internal practice is the cleanest example of compounding at the code level:

> "Close to 100% of our tests are written by Claude," and Anthropic engineers write **100% of their lint rules from PR comments**: when Claude generates code that triggers a review comment, they ask Claude Code to write a lint rule that catches it automatically going forward. — Boris Cherny / Cat Wu (*Inside Claude Code*, 2025-10-29)

"If they're bad, we just won't commit it." The economics: writing the lint rule once is cheap; reviewing the same issue manually on every future PR accumulates indefinitely.

## Stop hooks turn a stochastic agent into a deterministic loop

Hooks run at defined lifecycle moments and can enforce guardrails:

> "Define a stop hook that's like: if the tests don't pass, return the text 'keep going'… you can just make the model keep going until the thing is done. With scaffolding you can get these deterministic outcomes [from] a non-deterministic thing." — Boris Cherny

The human reviews the finished output, not each iteration. (Hooks were exposed precisely because users wanted custom integrations like Slack pings — "we didn't want to build the integration ourselves, so we exposed hooks.")

## The compounding store: learnings files, not a bloated config

The infrastructure for capturing learnings is covered in [03 · Context Engineering](03-context-engineering.md): categorized markdown files with front-matter, retrieved on demand by a `best-practice-researcher` sub-agent, so accumulated knowledge is available without polluting the context window. The discipline is to keep `CLAUDE.md` as a lean job description and push situational learnings into searchable files.

## "You can only compound as much as you plan"

A field observation from Every's consulting work: one engineering org was good at delegate/assess/compound but kept **skipping the plan phase**, so they "weren't going very far" and hit the same issues repeatedly.

> "You can only really compound as much as you plan." — Natalia (*How We Built 'Claudie'*, 2026-02-04)

Compounding without planning just accumulates fragile layers. The corollary discipline: **build incrementally and test end-to-end before adding the next layer.** Austin's cautionary tale — he tried to one-shot a full Discord-bot go-live, "unleashed it," and it broke the moment a user triggered an unbuilt feature. "Whenever I'm like 'looks pretty good, let me keep going,' and then something breaks… it's because I didn't actually use or understand it end to end before moving on." Kieran: "It will bite you."

## Self-improving automations (the frontier)

The most autonomous version is scheduled agents that improve the system without you:

- A Codex automation picks a **random file** and finds/fixes a subtle bug, several times a day — catching real latent bugs off the critical path that humans would never look for.
- Another "looks at the PRs you've done in the past day… and tries to ship a fix before anyone's noticed that you shipped a bug." (*OpenAI's Codex This Model Is So Fast*, 2026-02-18)

Yohei Nakajima's BabyAGI 2.0 takes self-improvement further — given "scrape TechMeme," it *wrote its own scraping tool, failed, and rewrote it twice until it worked.* But it reveals the current gap: it **throws working tools away** instead of saving them. "The right way is, if it creates a tool and it works, we should be storing the one that worked and reusing it next time" — the property where agents "only ever have to get it right once." That reuse layer is where compounding is still immature. (*Building AI That Builds Itself*, 2024-10-23)

## Skills and "keep the logic in prompts"

A durable architectural principle (Felix, who built Claude Cowork at Anthropic):

> "The more generalizable the tool is, the more you will benefit from improvements on model intelligence… [model intelligence improves] much faster than your ability to churn out additional tools." — *Vibe Check: Claude Cowork* (2026-01-13)

So keep logic in **skills and prompts** rather than hard-coded tool features — your workflow then rides free on model upgrades. Skills double as reusable context: Andrew Wilkinson built a copywriting skill encoding his preferred books and tone ("whenever I ask it to write copy, it just knows all the books I like"), and syncs Claude Code state across all his Macs with a cron job.

## Build scaffolding you fully expect to delete

Compounding doesn't mean hoarding. Anthropic deliberately builds throwaway scaffolding:

> "We build most things that we think would improve Claude Code's capabilities, even if that means we'll have to get rid of it in 3 months. If anything, we hope that we will get rid of it." — Cat Wu

Boris deleted ~2,000 tokens of system prompt because "Sonnet 4.5 doesn't need it anymore, but Opus 4.1 did." The scaffolding that remains is the scaffolding that still adds value; prune as models improve.

## Tensions & open questions

- **Compounding is the hardest, least-solved part — by the creator's own admission.** Kieran, who built the plugin, repeatedly says it's "still the hardest part for everyone" and "not built to be the smartest possible system yet." Don't expect a finished playbook.
- **The failure mode is meta-procrastination.** Brandon's warning: the worst version is "spending so much time trying to identify every single issue but then not doing the actual work and building the muscle of actually doing it."
- **Team sharing is unsolved.** How one person's compounded learnings reach a teammate's agent is experimental (a Notion compound DB; plan-sharing via Proof). "Cannot be real time either, because it's too slow."
- **Pruning cadence is unsolved.** A solutions folder accumulates stale, contradictory, superseded learnings; no one describes a maintenance process.
- **Will fast models undo it?** With ultra-fast inference, some builders speculate a return to a single "mega-prompt" over elaborate multi-agent compounding — see [09 · Choosing Models](09-choosing-models-for-coding.md). The meta-skill is re-deriving your own workflow, not cargo-culting whatever's hot.

## How to apply it

- The **second time** you write a long prompt or hit the same mistake, encode it (a command, a lint rule, a learning file).
- **Compound right after a review**, while the context is fresh; record the reasoning and distill it into a rule.
- Use **stop hooks / evals** so agents self-correct to a deterministic finish.
- Keep logic in **skills/prompts** so you ride model improvements; build scaffolding you're willing to delete.
- **Plan before you compound** — and build incrementally, testing end-to-end before the next layer.

## See also

- [02 · Planning & Specs](02-planning-and-specs.md) — "you can only compound as much as you plan"
- [03 · Context Engineering](03-context-engineering.md) — the learnings-file store and retrieval sub-agent
- [05 · Verification, Review & Testing](05-verification-review-and-testing.md) — turning review learnings into lint rules and judges
- [08 · Tools, Harnesses & Form Factors](08-tools-harnesses-and-form-factors.md) — commands, skills, hooks, sub-agents as primitives

## Sources

Primary: *Inside Claude Code From the Engineers Who Built It* (Boris Cherny / Cat Wu, 2025-10-29); *Compound Engineering: Your Questions Answered Live* (Kieran Klaassen / Austin, 2026-02-24); *How Two Engineers Ship Like a Team of 15 With Claude Code* (2025-06-11); *How We Built Our AI Email Assistant Cora* (2025-06-26); *How We Built 'Claudie'* (Natalia, 2026-02-04); *OpenAI's Codex This Model Is So Fast* (2026-02-18); *Building AI That Builds Itself* (Yohei Nakajima, 2024-10-23); *Vibe Check: Claude Cowork* (Felix / Anthropic, 2026-01-13); *How Andrew Wilkinson Uses Opus 4.5* (2026-01-21).
