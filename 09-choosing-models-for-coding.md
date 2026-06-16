# 09 · Choosing & Using Models for Coding

> Models are not interchangeable, and their differences are not only "how smart." The corpus's practitioners pick models by *personality* (precise vs. industrious), by *task* (hard bug vs. creative gap-filling vs. production agent), and increasingly by *speed* — and they treat model choice as a moving target you re-evaluate every few months.

*Part of the [Coding with AI wiki](index.md). Prev: [08 · Tools, Harnesses & Form Factors](08-tools-harnesses-and-form-factors.md) · Next: [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md)*

> Note on model names: the corpus spans late 2023 to mid-2026 and references many model versions (GPT-4, o1/o1-pro, o3, Opus 4 → 4.6, Sonnet 4.5/4.6, Codex / GPT-5.x, Spark, Gemini 3). The *specific* rankings below are snapshots; the durable lessons are the selection *principles*.

---

## Capability determines the form factor

The most important framing comes from Scott Wu (Cognition):

> "The correct form factor — the correct interface — is a pretty tight function of the capabilities" of the underlying model. — *Cognition's CEO on What Comes After Code* (2025-09-24)

A fully agentic CLI would have been useless on GPT-3.5; it works now because models like Opus 4 can sustain autonomous multi-step work. This means **your whole workflow has a model-capability assumption baked in**, and a calibration you set a year ago may be stale. It's also why a step change can be a *product/harness* change rather than a raw-IQ change — Paul Ford argues Opus 4.5 in Claude Code is mostly "a layer of agent-style thoughtfulness… constantly evaluating its own outputs," not "9,000 times better." Dan Shipper's experience of the same release: "the first time I've been able to vibe-code and it just keeps going without tripping over itself… if there are errors, it fixes it."

## Model personalities (and which to reach for)

The recurring, concrete distinction practitioners draw:

- **Codex / GPT-5.x — the precise "senior engineer."** "The code it generates feels like some senior engineer wrote it." It does *only* what you asked and "doesn't go off and do its own thing." Good for rigorous work and number-checking. (Naveen Naidu; the Spark blind test had Opus pick Codex's implementation as "more comprehensive… thought through corner cases.")
- **Claude Code / Opus — the industrious, sometimes-sycophantic "junior."** "It's just going to keep banging its head against the wall until it figures it out" (Dan Shipper). More creative gap-filling and design aesthetic; preferred for *vibe* coding. Caveat: more sycophantic ("you're absolutely right") and more token-hungry.
- **Opus 4.5/4.6 — for the hard bug and the creative build.** Andrew Wilkinson keeps Gemini 3 / ChatGPT as a fallback "for a really hard bug fix that Opus 4.5 can't get," but reaches for Claude Code first because the "tooling is just not as good" elsewhere and "Claude Code was so much better at understanding my intent and doing long-run tasks."
- **Thinking models for genuinely hard code.** For low-level Swift / 1980s-era CGEvent APIs with "bare minimum documentation," "GPT-5 high is so good at ripping through all this complex code… that's one of the biggest unlocks for why I didn't hire another developer." (Naveen) o1-pro was the earlier go-to for "deepest planning/thinking/strategy."

A notable defection signal: Kieran, an early Claude Code devotee, is now "~50/50 Opus 4.6 vs. Codex 5.3" for building Cora — his first OpenAI coding use since GPT-4 — because OpenAI leaned into a friendlier, more creative side.

## Sonnet 4.6: the same intelligence pushed down a price tier

The "Live Vibe Check" of Sonnet 4.6 (2026-02-18) decodes a deliberate vendor strategy: **keep tier prices fixed and push each tier's performance up** — release an Opus, then a Sonnet that matches the prior Opus.

- Stated pricing: **Opus = $5 / $25 per 1M input/output tokens; Sonnet = $3 / $15** — "not quite half, but close."
- Verdict: feels about as smart as Opus, *not* dramatically faster, at ~half the price. Found a real failure mode — "both too cautious and too eager" (asked permission to name a worktree, then jumped straight into rebuilding a homepage instead of planning).
- The killer use case is **not your own coding but powering production agents**: their ghostwriter product was burning "$500–$1,000 a day in token cost… completely out of control"; a cheaper Opus-class model makes such agents viable. "It just got cheaper on its own."

## Speed is a form of intelligence — and it inverts the bottleneck

The Spark vibe check (2026-02-13) tests an ~1,000-tokens/sec model on Cerebras hardware:

> "Speed is a form of intelligence… it's not just what is the smartest; speed is a factor." — Dan Shipper

Consequences when generation becomes near-instant:

- **Tool calls become the slow part.** "Normally the tool calls are the fast parts, but now the tool calls are the slow part… you only need to write code when you need to be more deterministic." (Kieran)
- **Less multitasking, more flow.** Faster models pulled the Codex team *back* toward synchronous work — "you cannot delegate understanding, so speed there is a real advantage." (This complicates the long-leash gospel — see [04 · Delegation](04-delegation-and-the-leash.md).)
- **A possible mega-prompt revival.** "We're kind of back to the master prompt… if you have a very good master prompt, it's the fastest output." Sub-agents *slowed Spark down* in testing (~4 min vs. ~1.5 min) — so don't add orchestration reflexively (see [07 · Parallelism](07-parallelism-and-agent-fleets.md)).
- **Precompute → on-demand.** "Maybe you never do the summary until you look at it, because it's so fast." Free, instant inference changes production architecture.
- **Human consumption is the new limit.** "Now that we can generate at this speed, we need to be able to *consume* at this speed, and the UI affordances haven't been built for that yet." It even produced fast output so disconcerting that vendors *slowed it down in the UI* "just so you can see the words come in a little smoother."

Spark's trade-off is stated plainly: not as smart as Codex 5.3 or Opus 4.6 — "not the model you should use to figure out a nasty bug." Use fast models for interactive, brainstorming, prototyping, and changelog-style work.

## Cross-model verification

Because models have different strengths and blind spots, a common pattern is to **build with one and check with another**: "Opus builds the financial-story P&L, then Codex 5.3 checks the numbers" — which caught a real Opus error. Blind A/B's are also used to pick implementations (have a third model review two candidates without knowing which is which). See [05 · Verification](05-verification-review-and-testing.md).

## Benchmark and migrate — and don't over-optimize for today's model

Two converging disciplines:

- **Evals make migration tractable.** When a new model ships, your eval suite tells you immediately whether it breaks anything — "without evals, model migration is guesswork." (See [05](05-verification-review-and-testing.md).)
- **Sometimes the cheaper model is also better.** "See what it does when a new model drops. Is the new model better? Is the cheaper model better?… sometimes the cheaper model is also better." And capability gets ~10× cheaper for the same quality in roughly three months — "performance gains for free" — so "don't try to solve problems that's going to be one-shotted by the model in three months." (Cora team)

A practical build trick from Yohei Nakajima: **scaffold with a cheap model, then upgrade.** "I build with [a cheap model] to get the framework working, and when the framework doesn't error I'll upgrade the model."

## "No bad models anymore" — but pin your stack

By early 2025, Kieran: "there are no bad models anymore." The differentiator shifts to *intent-understanding, tooling, and fit* rather than raw capability. Still, constraining the model helps: Geoffrey Litt's custom GPT pins "React + TypeScript + Tailwind, generate all the code in a single file… this is really important to constrain what it's doing" so output drops straight into a template. And watch context limits — Opus 4.6 was reported token-hungry with a bug where running out of context failed to auto-compact, which is exactly when the 1M-token context window earns its (paid) keep.

## Tensions & open questions

- **"AGI in programming" vs. "a better product."** Andrew Wilkinson (echoing Amodei: "there will be no programmers in 2–3 years") vs. Paul Ford ("not a giant step change in the LLM… they added agent-style thoughtfulness"). The Sonnet 4.6 result — same intelligence, lower price tier, no magic — partly supports Ford.
- **Vendor competitive claims are self-serving.** OpenAI's "5.3-Codex beats every model / Anthropic losing steam" and Anthropic's harness-pairing arguments are both pitches; treat benchmark superlatives skeptically.
- **Speed vs. intelligence may be a *new category* of model,** with pricing and positioning still unsettled (Spark launched pro-only).

## How to apply it

- Match the model to the **task**: precise/senior (Codex/GPT-5.x) for rigor and number-checking; industrious/creative (Opus) for hard bugs, gap-filling, and design; thinking models for genuinely hard low-level code; **fast** models for interactive/prototype/brainstorm work (not nasty bugs).
- Use a **cheaper Opus-class model to power production agents**; reserve the top tier for your own hard problems.
- **Cross-check** important output with a second model.
- Keep an **eval suite** so you can migrate the day a new model drops; re-test cheaper models — sometimes they win.
- **Scaffold cheap, upgrade later;** don't hand-optimize around a limit the next model removes.
- Pin an **opinionated stack** to constrain output, and watch context limits.

## See also

- [08 · Tools, Harnesses & Form Factors](08-tools-harnesses-and-form-factors.md) — the harness side; harness+model pairing
- [04 · Delegation & the Leash](04-delegation-and-the-leash.md) — how speed re-collapses async into sync
- [05 · Verification, Review & Testing](05-verification-review-and-testing.md) — cross-model checks; choosing a judge model
- [07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md) — when orchestration overhead beats fast single-model runs

## Sources

Primary: *Cognition's CEO on What Comes After Code* (Scott Wu, 2025-09-24); *How Andrew Wilkinson Uses Opus 4.5* (2026-01-21); *Why Opus 4.5 Just Became the Most Influential AI Model* (Paul Ford, 2026-12-03); *Live Vibe Check: Testing Claude Sonnet 4.6* (2026-02-18); *Live Vibe Check: OpenAI's Super-fast Spark Model* (2026-02-13); *One Developer Got Thousands of Users* (Naveen Naidu, 2025-09-17); *How We Built Our AI Email Assistant Cora* (2025-06-26); *Building AI That Builds Itself* (Yohei Nakajima, 2024-10-23); *You Can Build an App in 60 Minutes with ChatGPT* (Geoffrey Litt, 2024-01-10).
