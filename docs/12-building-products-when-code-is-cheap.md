# 12 · Building Products & Businesses When Code Is Cheap

> If anyone can build the software, building it stops being the moat. The corpus's product lessons all follow from that: cut more than you add, ship a rough V1 fast, design for the model 12–18 months out, and find defensibility in distribution, taste, depth, or business model — because the code itself is now nearly free.

*Part of the [Coding with AI wiki](index.md). Prev: [11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md) · Next: [13 · Beginners & Democratization](13-beginners-and-democratization.md)*

---

## "Building is the easy part now"

Mike Krieger had Claude rebuild Instagram's predecessor, feature-complete, in ~2 hours. The constraint moved entirely off implementation:

> "The models today are good at adding features. They're not necessarily good about figuring out what to cut out of the product." — Mike Krieger, *Building Is the Easy Part Now* (2026-03-25)

Andrew Wilkinson built a personal email client matching his exact workflow in about a week — "anyone who's technical knows how astounding that is and how frustrating it is to build an email client" — versus Superhuman's ~5 years in beta.

## The new trap is over-building, not under-building

When every feature is one cheap PR away, products bloat. The recurring failure mode:

- **The "indoor tree."** Going zero-to-end in hours skips the forces that build product sense and ships "very generic products unlikely to break out." "Just because you can doesn't mean it should be in the first version." (Krieger). His Bourbon postmortem: "our biggest mistake was adding functionality over time rather than deleting it."
- **"Vegetables vs. wham."** Codex's Embiricos: "you can implement so many features that they start to crowd in on each other… you've done that 50 times and the product is way too bloated and no one's stepped back… we need new product-management hygiene." (Internal codenames: hard bugs = "vegetables," easy bugs = "wham.")

The discipline is **deletion**, which is socially and technically harder than addition. Granola's example (from the broader corpus): one year of beta feedback followed by *cutting* 50% of features. "If the answer to 'why isn't this in the product?' is 'we haven't built it yet,' that's not a moat — it's a roadmap."

## Rewrites are cheap now — Brooks's warning is (partly) retired

> "It's no longer a year-long rewrite that might have killed a company like Netscape… these are days." — Mike Krieger

You can throw out "half your product" every 3–6 months, and have the model diff V1 against V2 to catch what you dropped. Ship the minimal V1 fast (Cowork went 0→research-preview in ~10 days; "anyone coming up with the PRD in two weeks — nope, you ship the whole thing in a week and a half"). Lean-startup intuitions survive, "just at a different time scale."

## Dogfood: only build what you actually use

The simplest quality and focus discipline in the corpus:

> "We won't build anything that we don't use all the time… or it'll never launch." — Naveen Naidu, on Every's Studio (*One Developer Got Thousands of Users*, 2025-09-17)

Monologue reached "internal market fit" with ~13 Every teammates using it daily before public launch — and was "the first product ever made *using* that product." Danny Aziz (Spiral) makes the same point about why simplicity holds: "everybody is using the stuff we build every day… we're not building for people who are different than ourselves," which prevents over-building.

## Distribution is the scarce resource, not code

Naveen's framing is blunt about where his real advantage came from:

> "Dan did really hard work for the last seven years and I'm just coming in." — and the **bundle as moat**: "people subscribe to Every because they want it to be their last subscription outside of a ChatGPT or Claude," so "don't worry too much about copycats."

The skill that compounded for him wasn't any one app — it was "able to build apps really fast and then only build that one single core feature… and then post to Reddit/X for eyeballs → sales." The corollary anti-pattern he repeated twice: building for months without shipping ("I worked on one product for six months, didn't even release; no one really used it"), fixed by a self-imposed "one experiment per week" KPI.

## Moats when intelligence is cheap

The corpus is candid that the old moats (engineering complexity, data) are weaker, and even insiders say "I don't know what the moats are anymore" (Josh Miller). The viable theses, pick-and-invest:

- **Taste / conviction / "vibes."** Products built from genuine conviction feel distinctive and are hard to reverse-engineer; reactive products "feel derivative." Real only if protected from feature-checklist drift.
- **Relationship capital** (Henrik Werdelin): depth (the customer feels seen), density (you belong in the community), durability (permission to expand). "Define yourself by *who you serve*, not what you ship" — BarkBox's next product could be a dog airline, not a cat box. Durability test: "you can imagine a Nike hotel; it's hard to compute a Hilton shoe."
- **Deep vertical expertise + the refusal gap** (Mike Maples / Vicente Silveira / Guillermo Rauch): the "GPT wrapper" critique only holds when the wrapper is shallow. "Every refusal of a general AI is a startup opportunity" — a domain-specific product can go where a general model must hedge (Open Evidence, used by 250k medical professionals). And **"underdo" the incumbent**: "I'm going to under-UI you and beat you in functionality" by shedding the complexity mature SaaS accretes.
- **Business-model counter-positioning** (Maples): charge on *outcomes*, which seat-based incumbents structurally cannot copy. Compete for the labor-cost bucket, not the software-spend bucket.
- **Brand / distribution / hardware lock-in** (Andrew Wilkinson): with code near-free, "your moat has to come from a brand or a distribution mechanism or hardware lock-in." Tiny has "slowed down on buying technology companies" because so much SaaS is now "thin wrappers — a database call or an AI call."

## "SaaS apocalypse" or market expansion?

Matt Colyer (Figma) is the deliberate counterweight to the doom framing:

> "Software companies build more than just code… there's a reason I pay for Gmail to run my email… I'm buying *more* software these days than ever, because I'm going to pay somebody else to run my agent for me." — *The SaaS Apocalypse Is a Goldmine* (2026-06-03)

His thesis: if the developer population goes from tens of millions toward a billion, the total volume of software grows, and **more software means more demand for the tooling/infrastructure layer.** The operations-and-maintenance value (running it reliably) is the durable, paid-for part — not the code. Stripe's data backs the "net-new spend" view: top-100 AI companies reach $30M ARR ~3× faster than the 2018 SaaS cohort, mostly on net-new spend so far.

## Pricing follows marginal cost

Because inference has real marginal cost, pricing migrates from per-seat to **usage** and **outcomes** (see [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md)). Stripe expects per-seat developer/enterprise licenses to roughly halve within months; a **token-billing** product tracks and prices in real time to underlying token costs (plus markup) so wrappers' margins don't evaporate when model prices move. Even Linear, "the perfect SaaS business for this era" (sticky, no token costs), had to add **usage-based billing** the moment it shipped its own coding agent.

## Build for the capability curve

Because model capability improves on a known trajectory, building around today's constraints often means building something obsolete when the constraint lifts:

> "Don't solve problems that won't be problems 12 months from now." — Chris Pedregal

The Browser Company killed nine months of memory work because the model capability wasn't there, then shipped it in six weeks when it arrived. The moat isn't the implementation of the memory system (anyone could ship it once the capability landed) — it's the vision that kept the team trying. Invest in what survives commoditization: relationships, domain expertise, brand, business model.

## Ship rough and watch (the "construction site")

A deliberate strategy from shipping Cowork: release the rough thing as a *separate, less-clicked surface* — "a construction site… letting you into our kitchen" — so fewer people hit it but you iterate with real users. "It's very hard to figure out a great product in isolation… even the first iPhone was missing things we consider table stakes." (See [08 · Tools & Harnesses](08-tools-harnesses-and-form-factors.md) on agent-native shipping cadence.)

## The contrarian operating model

Every's Studio runs an explicit experiment: "**Don't raise any money and don't hire anybody, do it all yourself, and we'll support you with ideas and resources**" — enabled by a profitable media business as the foundation and by AI making products "so much cheaper and faster to make." Capital discipline changes behavior, not just runway: lacking "we can burn this because we'll raise more" forces creativity. The open question they state plainly: "does the model work?" Monologue's target — $1M ARR as a solo-built, *multi*-million (not billion) outcome — is the deliberately un-VC shape: "building from your front foot instead of trying to dig yourself out of a hole."

## Tensions & open questions

- **The moats are genuinely uncertain.** Insiders disagree (taste/vibes vs. business-model counter-positioning vs. AI-durable verticals) and admit they don't know which hold at scale.
- **"Latent software" optimism vs. reproducibility reality.** Paul Ford's "PDFs and forms become real software on demand" vs. regulated buyers needing deterministic, traceable behavior (see [14 · Pitfalls](14-pitfalls-limits-and-risks.md)).
- **Does the "don't raise, don't hire" model generalize** beyond a company with an existing profitable media/distribution engine? Unproven.

## How to apply it

- Spend your scarce judgment on **what to cut**, not what to add; treat **deletion** as a core discipline.
- **Dogfood** — build what you use; reach "internal market fit" before launch.
- Ship a **rough, minimal V1 fast** and iterate with real users; rewrites are cheap now.
- Pick **one moat thesis** (taste, relationship capital, vertical depth, business model, distribution) and invest in it specifically.
- **Price for marginal cost** (usage/outcomes) and protect wrapper margins as token costs move.
- **Design for the model 12–18 months out;** don't solve problems the next model removes.

## See also

- [11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md) — where scarce value migrates
- [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md) — usage/outcome pricing, the agent economy
- [13 · Beginners & Democratization](13-beginners-and-democratization.md) — bespoke/personal software as a product category
- [14 · Pitfalls, Limits & Risks](14-pitfalls-limits-and-risks.md) — bloat, "app glaze," reproducibility, slow adoption

## Sources

Primary: *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *OpenAI Launches Codex* (Alexander Embiricos, 2025-05-16); *One Developer Got Thousands of Users* (Naveen Naidu, 2025-09-17); *How We Incubate and Launch New Products With AI* (Danny Aziz / Brandon, 2024-11-27); *The SaaS Apocalypse Is a Goldmine* (Matt Colyer, 2026-06-03); *What the Agent Economy Looks Like From Inside Stripe* (Emily Sands, 2026-04-29); *How Linear Turned AI Agents Into First-class Users* (2026-04-01); *How Andrew Wilkinson Uses Opus 4.5* (2026-01-21); *Vibe Check: Claude Cowork* (2026-01-13); the corpus's moats material (Werdelin / Maples / Silveira / Miller / Pedregal).
