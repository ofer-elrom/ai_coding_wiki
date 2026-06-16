# 10 · Agents as Users: Building Software & Infrastructure for Agents

> The internet was built on the assumption that the actor on the other end is a person. As agents become first-class users of software, every layer — interfaces, identity, auth, payments, even how you expose an API — needs to change. This doc is about building *for* agents, not just *with* them.

*Part of the [Coding with AI wiki](index.md). Prev: [09 · Choosing Models for Coding](09-choosing-models-for-coding.md) · Next: [11 · The Changing Role of the Engineer](11-the-changing-role-of-the-engineer.md)*

---

## Treat agents as first-class users

Linear's bet is to be the **context/intent layer upstream of coding agents**, not the agent itself:

> "Linear becomes a system for guiding the agents and building this context… because of that, we now have most of the coding agents integrated with Linear." — Karri Saarinen, *How Linear Turned AI Agents Into First-class Users* (2026-04-01)

The mechanism is telling: Linear published good docs and an open agent platform, and **coding agents self-built the integrations** — Linear didn't have to. Its core pitch against generic tools is context: "I have to really explicitly tell [a generic] agent what context to bring… the value with Linear is that the context lives there and we inject it smartly… we don't spam the context windows." Saarinen expects a multi-agent world ("everyone will have many agents, and companies will build their own"), already visible in customers like Coinbase building homegrown coding agents that interoperate with Linear.

A related design coinage from building Proof: **"AX is just as important as UX"** — agent experience. And the way to optimize it is delightfully direct: "ask the agents… how would you have made this better? Why did you get confused?" (*How We Use Proof*, 2026-03-11)

## Exposing an API to an LLM is an unsolved design problem

Alex Rattray (Stainless, ex-Stripe API team) is blunt that naïve MCP doesn't work:

> "We haven't figured out how to expose an API ergonomically to an LLM in the same way we've figured out how to expose it ergonomically to a Python developer… I can go learn to be a Python developer. I can't really learn to think or see like an LLM." — *MCP Servers Teaching AI to Use the Internet Like Humans* (2025-10-01)

Dumping a full API (Stripe's hundreds of endpoints) into MCP tools "burns through your entire context budget… and is confusing to the model." His design rules: **few, hand-crafted tools** ("one specialized tool to look up a customer and refund their transaction"), precise tool names, small input schemas, and trimmed responses (they feed the model a `jq` filter). Stainless offers three modes — all-endpoints (small APIs), a filtered subset, or **dynamic mode** (3 tools: list/get/execute endpoint) that scales but costs 3 model turns per action and is lossier.

And feedback is a black box: he proposes a first-class **"send feedback" tool** so the server learns when its output was useless — "if a user says out loud 'oh man, that was useless garbage,' now at least the MCP server is going to find out about it."

## "The future of AI is cyborgs": code execution over tool sprawl

Rattray's bigger bet is that the dominant pattern becomes **the model writing code**, not calling dozens of tools:

> "The future of AI is cyborgs — part LLM, part traditional CPU code… the model has two tools: one to execute code, where it has a text box of 'put in some TypeScript and use this API's SDK,' and one to search docs. This is really easy for models — they're really good at writing code."

Why it wins: pagination and loops run server-side in a CPU, so "the context impact of doing a whole bunch of paginated list requests is zero… only the final `console.log` returns." And a **typed SDK lets a type checker catch wrong calls** before they execute — "without a typed library, the LLM will make a wrong API request some percentage of the time; the code-execution tool can run a type checker and say 'that method doesn't exist, you might want payment intents.'" Enduringly useful generated code should be committed to the repo — "that's what software teams do all day." (This is the same intuition behind ultra-fast models replacing deterministic steps — see [09 · Choosing Models](09-choosing-models-for-coding.md).)

## Identity, auth, and trust for non-human actors

Security cannot be bolted onto the agent layer; it has to live deeper:

- **Security at the API layer.** "People should be using OAuth with granular permissions, with proper scopes… there are limitations to OAuth scopes, and it's pretty hard to build." Limiting what MCP *exposes* is not security, because the underlying API can still do anything. (Rattray)
- **Locked-down sandbox egress.** For code-execution safety, "ensure there's no network connections allowed out of that sandbox other than, in this case, `api.stripe.com`." (See [14 · Pitfalls](14-pitfalls-limits-and-risks.md).)
- **Agents need identities.** Kevin Scott (Microsoft): "we need agents to have identities so you can build entitlement systems… this agent is acting on behalf of this person." MCP today lacks anything like the browser same-origin policy.
- **Public-only interaction as a lightweight trust layer.** Every's rule for agent-to-agent contact (see [07 · Parallelism](07-parallelism-and-agent-fleets.md)).

## The Claude platform's trajectory and the "outcome and a budget" north star

Anthropic's platform team describes climbing abstraction levels: completion endpoint → tool calling → stateful sessions → "Claude on a computer with memory," each step "in the pursuit of helping you get the best outcomes with the least work." Two load-bearing claims:

- **Harness and model are paired,** not generically hot-swappable; early primitive choices (file systems, tool-call format) create path dependence (see [08 · Tools & Harnesses](08-tools-harnesses-and-form-factors.md)).
- **The end state compresses to "an outcome and a budget."** "Maybe everything should compress down to an outcome and a budget… everything else should be figured out for you." The running joke — "Claude, make me a billion dollars; your budget is $10; no mistakes; go" — names exactly what's still missing: a human-defined spec the system can re-grade itself against. (*The Secrets of Claude's Platform*, 2026-05-08)

They also flag **agent lifecycle** as a real problem: agents go stale (no owner, old model), so there "needs to be an end-of-life cycle for agents" — they joke about agent "funerals" and ship skills to help upgrade an agent to a new model ("that migration is a breaking change"). And the hardest part isn't harness engineering — it's the **infrastructure wall** (long-running async agents, sandboxing, transcript storage) — see [08](08-tools-harnesses-and-form-factors.md).

## The agent economy: payments, fraud, and pricing

Emily Sands (head of data & AI, Stripe) describes the economic layer rebuilding itself for agents (*What the Agent Economy Looks Like From Inside Stripe*, 2026-04-29):

- **"Free compute is the new CAC."** Because every prompt and API call has real cost, fraudsters now steal *compute*, not just card numbers. Free-trial abuse 4×'d in six months; for one large AI company Stripe blocks **250,000 fraudulent free trials a week**; ~15% of legitimate AI-company card transactions are virtual cards (so you can't just block them). Fraud moved from a transaction problem to a full-funnel customer problem — Radar now scores at *signup*, not just checkout.
- **Machines are becoming users of developer infrastructure.** "LLM traffic to Stripe docs is up 10× year-over-year" while human doc traffic is flat. The "developer" is now a broader persona — a non-technical founder on Replit, a coding assistant, or an agent provisioning infra.
- **Pricing migrates from seats → usage → outcomes.** "I'd be super surprised if six months from now we have half the seat-based licenses we have today" — because if agents make each developer ~10× more productive, pegging revenue to headcount is self-defeating. A **token-billing** product prices in real time to underlying token costs so wrappers' margins don't evaporate (see [12 · Building Products](12-building-products-when-code-is-cheap.md)).
- **New payment primitives.** The **shared payment token (SPT)** passes buyer credentials securely from agent to merchant (keeping the merchant the merchant-of-record and carrying the fraud score); the **Agent Commerce Protocol** (co-created with OpenAI; also used by Microsoft Copilot, Meta) lets a merchant integrate once and toggle on across many agent storefronts. Volume is "relatively small" but growing.

A recurring meta-pattern across this batch: **AI makes X cheaper but Y more burdensome.** Coding gets easier, but code review gets heavier; building gets easier, but provisioning is still manual — which is why Stripe Projects exists ("coding is getting easier a lot faster than the setup is getting easier").

## Tensions & open questions

- **Generic, model-agnostic surfaces vs. harness+model pairing.** Stainless and Linear want surfaces that work across models; Anthropic argues the harness and model are tightly paired. Unresolved and consequential.
- **MCP is promising but rough.** Independent reports of MCP servers that disconnect, flaky installs, and a strong preference for "a skill with a CLI" over MCP (Wilkinson; Kieran). MCP may be early-HTTP for agents (Kevin Scott's analogy) — or it may lose to code execution.
- **Auth/identity/trust for non-human actors is genuinely unsolved** and load-bearing for the whole agent-economy thesis.

## How to apply it

- If you build software agents will use, **publish good docs and an open integration surface** — agents will self-integrate.
- Design MCP with **few hand-crafted tools and trimmed responses**, or move to **code execution with a typed SDK** and locked-down sandbox egress.
- Put **security at the API layer** (OAuth scopes), not at the tool-exposure layer.
- Expect pricing to move toward **usage/outcomes**; protect wrapper margins as token costs move.
- Plan for **agent identity and lifecycle** (ownership, model upgrades, decommissioning) from the start.

## See also

- [08 · Tools, Harnesses & Form Factors](08-tools-harnesses-and-form-factors.md) — harness+model pairing; skills vs. MCP
- [07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md) — multi-agent orchestration and agent-to-agent trust
- [12 · Building Products When Code Is Cheap](12-building-products-when-code-is-cheap.md) — usage/outcome pricing, the operations moat
- [14 · Pitfalls, Limits & Risks](14-pitfalls-limits-and-risks.md) — sandbox egress, compute-theft fraud, non-human-actor security

## Sources

Primary: *How Linear Turned AI Agents Into First-class Users* (Karri Saarinen, 2026-04-01); *MCP Servers Teaching AI to Use the Internet Like Humans* (Alex Rattray, 2025-10-01); *The Secrets of Claude's Platform* (Angela / Caitlyn, 2026-05-08); *What the Agent Economy Looks Like From Inside Stripe* (Emily Sands, 2026-04-29); *We Gave Every Employee an AI Agent* (2026-04-08); *How We Use Proof* (2026-03-11); *Kevin Scott on the Future of Programming* (2025-05-21).
