# Coding With AI — A Practical Wiki

*Learnings about professionally writing code with AI, extracted from Every's "AI & I" podcast (hosted by Dan Shipper) and related Every shows.*

This wiki distills what working practitioners — the engineers who built Claude Code and Codex; the CEOs of Cognition, GitHub, Linear, Vercel, and Figma; and dozens of small teams shipping real products — actually say about building software with AI. It is organized as fourteen connected subject documents plus a synthesizing essay. Every claim is attributed; the disagreements and caveats are kept rather than sanded away.

**New here? Read the [synthesis essay](00-synthesis-coding-with-ai.md) first** — it ties the whole thing together in one argument. Then use the map and reading paths below.

---

## The map

**Start here**
- **[00 · Synthesis: Coding With AI](00-synthesis-coding-with-ai.md)** — the one essay that ties all fourteen subjects together: the big shift, the eight through-lines, the real disagreements, and how much to believe.

**The core operating system (how to work with coding agents day to day)**
- **[01 · From Writing Code to Directing Agents](01-from-writing-code-to-directing-agents.md)** — the paradigm shift: the AI sandwich, the three kinds of engineer, the manager / allocation-economy model.
- **[02 · Planning & Specs](02-planning-and-specs.md)** — the thinking happens before the code: plan mode, "fix at the lowest value stage," voice→PRD, throwaway dev.
- **[03 · Context Engineering](03-context-engineering.md)** — "every problem is a context problem": `CLAUDE.md` as a job description, learnings files, context rot, connectors over content.
- **[04 · Delegation & the Leash](04-delegation-and-the-leash.md)** — how much autonomy to give: leash length, sync↔async, yolo mode, automations — and why the real variable is *verifiability*.
- **[05 · Verification, Review & Testing](05-verification-review-and-testing.md)** — the new bottleneck: behavioral verification, reviewer sub-agents, the *assembled approval rubric*, and the tension that caps the whole ROI thesis.
- **[06 · Compounding Engineering](06-compounding-engineering.md)** — build systems that make the next task cheaper: commands, skills, hooks, lint-from-review, stop hooks.

**Scaling up (many agents, the right tools and models)**
- **[07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md)** — running many agents at once: sub-agents, map-reduce, orchestration patterns, the "ant death spiral."
- **[08 · Tools, Harnesses & Form Factors](08-tools-harnesses-and-form-factors.md)** — the harness is most of the product: agent-native architecture, skills over hard-coded tools, the unsettled form factor.
- **[09 · Choosing & Using Models for Coding](09-choosing-models-for-coding.md)** — model personalities (precise vs. industrious), speed as intelligence, cross-model verification, benchmark-and-migrate.
- **[10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md)** — building *for* agents: MCP design, code execution, identity/auth, and the Stripe-eye view of the agent economy.

**The bigger picture (people, products, limits)**
- **[11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md)** — skills, taste, careers, teams — and the judgment-development gap.
- **[12 · Building Products When Code Is Cheap](12-building-products-when-code-is-cheap.md)** — what to cut, cheap rewrites, distribution, moats, pricing, building for the capability curve.
- **[13 · Beginners & Democratization](13-beginners-and-democratization.md)** — non-engineers building real software: genuinely open, genuinely bounded.
- **[14 · Pitfalls, Limits & Risks](14-pitfalls-limits-and-risks.md)** — the honest counterweight: silent failure, bloat, security, reproducibility, and how to weigh the evidence.

---

## Reading paths

- **Engineer adopting agents:** [02](02-planning-and-specs.md) → [05](05-verification-review-and-testing.md) → [06](06-compounding-engineering.md) → [04](04-delegation-and-the-leash.md) → [07](07-parallelism-and-agent-fleets.md).
- **Non-technical, want to build:** [13](13-beginners-and-democratization.md) → [01](01-from-writing-code-to-directing-agents.md) → [02](02-planning-and-specs.md) → [03](03-context-engineering.md).
- **Founder / PM / exec:** [12](12-building-products-when-code-is-cheap.md) → [10](10-agents-as-users-and-infrastructure.md) → [11](11-the-changing-role-of-the-engineer.md) → [05](05-verification-review-and-testing.md).
- **Skeptic / risk-minded:** [14](14-pitfalls-limits-and-risks.md) → [05](05-verification-review-and-testing.md) → [00](00-synthesis-coding-with-ai.md).
- **Tool/model chooser:** [08](08-tools-harnesses-and-form-factors.md) → [09](09-choosing-models-for-coding.md) → [10](10-agents-as-users-and-infrastructure.md).

## The through-lines (in one breath)

The work moved **up a level** ([01](01-from-writing-code-to-directing-agents.md), [11](11-the-changing-role-of-the-engineer.md)) · leverage concentrates in the **plan** ([02](02-planning-and-specs.md)) · **context** is the real substrate ([03](03-context-engineering.md)) · **verification** is the new bottleneck and it caps the multiplier ([05](05-verification-review-and-testing.md)) · **compounding** beats one-off speedups ([06](06-compounding-engineering.md)) · **models keep moving**, so build for the curve ([09](09-choosing-models-for-coding.md)) · value migrates to **taste, judgment, distribution, depth** ([12](12-building-products-when-code-is-cheap.md)) · **agents become users**, so the software around them must change ([10](10-agents-as-users-and-infrastructure.md)).

---

## How this wiki was made

- **Corpus:** ~118 transcripts in the "AI & I" / "How Do You Use ChatGPT?" library (Every; host Dan Shipper), spanning late 2023 to mid-2026.
- **Scope:** roughly 40 transcripts directly about writing code / building software with AI were read in full and mined for concrete, attributable learnings; the rest of the library (writing, learning, personal use) was set aside.
- **Method:** each transcript was extracted into specific claims with verbatim quotes, speaker attribution, and credibility flags (vendor pitch / self-report / neutral), then organized under twelve recurring themes and written up as the fourteen subjects here. Disagreements and failure modes were preserved deliberately.

## A note on evidence (please read)

Almost everything here is **single-team, self-reported anecdote or live demo**, not controlled study — and the product being demoed is often the thing being sold. One recurring host (Dan Shipper) is also a builder/author in this space, so apparent "consensus" is sometimes one worldview repeated. Treat the **direction as well-supported and the magnitude as unproven** until you've measured it yourself. The full discussion is in [14 · Pitfalls, Limits & Risks](14-pitfalls-limits-and-risks.md) and the closing section of the [synthesis](00-synthesis-coding-with-ai.md).

---

## Cast of voices (selected)

**Tool & model builders:** Boris Cherny & Cat Wu (Claude Code, Anthropic); Angela & Caitlyn (Claude platform, Anthropic); Felix (Claude Cowork, Anthropic); Mike Krieger (Anthropic; co-founder, Instagram); Alexander Embiricos, Tibo Sottiaux & Andrew (Codex, OpenAI); Scott Wu (Cognition / Devin); Thomas Dohmke (GitHub); Kevin Scott (Microsoft); Guillermo Rauch (Vercel / v0); Karri Saarinen (Linear); Alex Rattray (Stainless); Emily Sands (Stripe); Matt Colyer (Figma).

**Practitioners & builders:** Kieran Klaassen, Natesh, Brandon Gell, Natalia, Naveen Naidu, Danny Aziz, Austin, Willie, Kate Lee, Mike Taylor (Every & products); Geoffrey Litt (Ink & Switch); Yohei Nakajima (BabyAGI / Untapped); Noah Brier (Alephic); Andrew Wilkinson (Tiny); Paul Ford (Aboard); Nick Dobos (Grimoire).

**Host / recurring author:** Dan Shipper (CEO, Every).

## Source episodes used

Grouped roughly by focus (titles abbreviated; all from Every's "AI & I" / "How Do You Use ChatGPT?" and related Every shows):

- **Claude Code & agentic workflow:** *Inside Claude Code From the Engineers Who Built It*; *Inside the Claude Code Workflow That 15× Their Output* / *How Two Engineers Ship Like a Team of 15*; *Compound Engineering: Your Questions Answered Live*; *Claude Code Can Be Your Second Brain*; *Vibe Check: Claude Cowork*; *How We Built Our AI Email Assistant Cora*; *How We Built 'Claudie'*; *The AI Sandwich*.
- **Future of programming / leaders:** *Cognition's CEO on What Comes After Code*; *GitHub CEO on the AI Coding Arms Race*; *Kevin Scott on the Future of Programming*; *Four Predictions for How AI Will Change Software in 2026*; *Building Is the Easy Part Now* (Mike Krieger).
- **Vercel / prompting / Codex:** *Vercel's Guillermo Rauch on AI and the Future of Coding* (Ep. 47 and the "Best of" re-release — same interview); *Is Prompting the Future of Coding* (Nick Dobos); *OpenAI Launches Codex*; *OpenAI's Codex This Model Is So Fast*.
- **Beginners & fast builds:** *What Happens When Beginners Start Building With Claude Code*; *You Can Build an App in 60 Minutes with ChatGPT* (Geoffrey Litt); *One Developer Got Thousands of Users* (Monologue); *He Built an AI App With 10,000 Signups*; *Building AI That Builds Itself* (Yohei Nakajima).
- **Agents as users / infrastructure / economy:** *How Linear Turned AI Agents Into First-class Users*; *MCP Servers Teaching AI to Use the Internet Like Humans*; *The Secrets of Claude's Platform*; *What the Agent Economy Looks Like From Inside Stripe*; *We Gave Every Employee an AI Agent*.
- **Build-is-cheap economics / org / teams:** *The SaaS Apocalypse Is a Goldmine* (Figma); *We Automated Everything With AI and Tripled Our Headcount*; *How Every Edits in the Age of AI*; *How We Incubate and Launch New Products With AI*; *How We Use Proof*.
- **Models for coding / case studies:** *How Andrew Wilkinson Uses Opus 4.5*; *Why Opus 4.5 Just Became the Most Influential AI Model* (Paul Ford); *Live Vibe Check: Testing Claude Sonnet 4.6*; *Live Vibe Check: OpenAI's Super-fast Spark Model*; *Meet the Team Behind Monologue*.

Additional context drew on adjacent episodes in the library (Tyler Cowen on jobs; the moats/pricing conversations with Mike Maples, Henrik Werdelin, Vicente Silveira, Chris Pedregal; the evals material from Spiral and the audience-simulator episode).

---

*This wiki synthesizes third-party podcast content for learning purposes; quotes are transcribed from the source recordings and attributed to their speakers. Model names, prices, and tool rankings reflect the dates of the recordings and move quickly.*
