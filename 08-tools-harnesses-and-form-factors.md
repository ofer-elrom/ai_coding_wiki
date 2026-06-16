# 08 · Tools, Harnesses & Form Factors

> The same model behaves very differently depending on the harness around it. The corpus's strongest tooling claims are about **what surrounds the model** — bash access, generalizable tools, skills, plan modes, agent-native architecture — and about a form factor that is still very much in flux.

*Part of the [Coding with AI wiki](index.md). Prev: [07 · Parallelism & Agent Fleets](07-parallelism-and-agent-fleets.md) · Next: [09 · Choosing Models for Coding](09-choosing-models-for-coding.md)*

---

## Give the model general tools, not bespoke ones

Claude Code's founding bet, per its creator:

> "The model just wants to use tools… we gave it bash and it just started writing AppleScript to automate stuff. I've never seen anything like this." — Boris Cherny, *Inside Claude Code* (2025-10-29)

The team adds and *removes* tools most weeks, pruning to reduce the model's choices and keep context clean — they unshipped the `LS` tool once bash permissions could enforce read restrictions ("a little less stuff in context"). They even moved off vector embeddings to **agentic search** (grep/bash): "you can get to the same accuracy level with agentic search" without the reindexing burden and security surface of a vector store.

## The harness is most of the product

Cora's team abandoned Cursor/Windsurf for Claude Code despite the same underlying models — the win was the harness:

> "Claude just takes it one step further by simplifying it by a factor of 10." — *Team of 15* (2025-06-11)

The value was being *general* (good at research, workflows, and code, not just code) plus a simpler UI — "just a text box… it works because the underlying model is so much more capable now." Anthropic's platform team quantifies the harness's leverage: for their memory launch, different harnesses "performed drastically differently" on the same eval suite — "you can hill-climb a tremendous amount by just harness engineering."

## Agent-native architecture

The design pattern several builders converge on (Every's framing, exemplified by Claude Code — "Claude Code in a trench coat"). Its principles:

- **Parity** — anything a user can do in the app, the agent can do.
- **Granularity** — tools live *below* the level of features; features are just prompts (slash commands, skills, sub-agents), so users can build their own features. "Anything you can do on your computer Claude Code can do… the features of Claude Code are actually just prompts." (Paul Ford / Dan Shipper)
- **Composability** and **emergent capabilities** — latent demand surfaces uses you didn't design, and you build for them.

Mike Krieger's deeper point: agent-native isn't only new power, it's "unlocking functionality that always should have been there." His negative example from Anthropic's *own* older product: Claude AI tells you the *steps* to add a doc to project knowledge instead of just doing it. "No, that should just be a thing that it can do natively." (*Building Is the Easy Part Now*, 2026-03-25)

## Keep logic in prompts and skills, not hard-coded tools

Felix, who built Claude Cowork:

> "I'm really leaning into skills… instead of writing MCP tools or a very specific harness, I just write skills. The more generalizable the tool, the more you benefit from model-intelligence improvements — which improve much faster than your ability to churn out additional tools." — *Vibe Check: Claude Cowork* (2026-01-13)

The core design decision is the **deterministic vs. non-deterministic split**: "break up workflows into the non-deterministic and the repeatable parts… the more repeatable something is, that's where it might make sense to write a tool." But every part you leave to model intelligence degrades if you pick a cheaper/dumber model. Skills are increasingly **portable across harnesses** (Claude Code, Codex, Cursor), and several builders now prefer "a skill with a CLI" over MCP (see [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md)).

## Harness and model are becoming *paired*, not hot-swappable

A sharp claim from Anthropic's platform team that cuts against the "generic harness, swap any model underneath" ideal:

> "The harness and the model get very paired… you probably [swap] at the layer of the agent — meaning the harness plus the model — rather than a really generic harness and hot-swapping everything underneath." — Caitlyn (*The Secrets of Claude's Platform*, 2026-05-08)

Early small choices (file systems, tool-call format) create **path dependence** that "locks the model's trajectory" and even its personality. This is a live disagreement with the Stainless/Linear view that surfaces should stay model-agnostic — see [10](10-agents-as-users-and-infrastructure.md).

## The form factor is unsettled

Strong opinions, no consensus:

- **The terminal isn't the end state.** "My prediction is terminal is not the final form factor." (Boris Cherny). Paul Ford notes Claude Code "has nothing to do with human beings — you can't put a civilian in front of that interface."
- **A GUI that isn't an IDE.** The Codex team built exactly this — "GUIs are great, IDEs are just the problem… there's something that's a GUI for programming that's not an IDE." They "speedran the TUI era and came back to GUIs" once agents run in parallel, and deliberately let *the model decide what UI to show* in the moment (plan mode shows an editable plan, not a free-form composer). (*OpenAI's Codex This Model Is So Fast*, 2026-02-18)
- **Collapse toward one interface.** Andrew Wilkinson and others predict tools collapse into "a single command line and then eventually a single voice." Scott Wu's contrarian note: "I actually don't know that CLI itself is the most important part of the experience" — what matters is the capability and "handing the reins over to your AI buddy."
- **The counter-lesson (Excel/Slack):** power users prize "deep familiarity over marginal productivity gains," so purpose-built surfaces often fail to displace chat. "You probably don't get B2B SaaS without Excel." (Dan Shipper / Felix)

## A snapshot tool tier-list (and why it's a snapshot)

Kieran's S-to-F ranking from mid-2025 — explicitly one person's vibes at a moment in time: **Claude Code = S, AMP = S, Cursor = A, Devin/Codex/Factory = B, Windsurf = C, Copilot = D.** The instructive part is the volatility: Windsurf was "A three weeks ago" and got downgraded only because it lacked Claude 4. "The entire coding landscape changes completely every three months… nobody's at the forefront." The durable skill is daily practice, not allegiance to a tool. (See [09 · Choosing Models](09-choosing-models-for-coding.md) for the model-personality angle, which is changing just as fast.)

## The real wall is infrastructure, not the harness

A surprise from the people who build these platforms:

> "Everybody hits an infrastructure wall… they're more expecting that the actual harness engineering is the part that's going to be harder. [But] if you boot a Claude Code session in a sandbox and your sandbox loses connection and dies, your whole agent dies… productionizing is just a freaking nightmare." — Caitlyn (*The Secrets of Claude's Platform*, 2026-05-08)

Long-running async agents, sandboxing, transcript storage, and agent lifecycle are where teams get stuck — which is part of why "run a hosted agent for me" is itself becoming a product (see [10](10-agents-as-users-and-infrastructure.md) and [12 · Building Products](12-building-products-when-code-is-cheap.md)).

## Tensions & open questions

- **Generic harness vs. harness+model pairing.** Anthropic argues pairing; Stainless and Linear want model-agnostic surfaces. Unresolved, and consequential for anyone betting a product on one side.
- **One interface vs. many.** Collapse-to-voice vs. "many agents, choice is part of the joy of being a developer" (Kevin Scott). The Excel/Slack lesson suggests chat is stickier than purpose-built surfaces.
- **How much tooling to give the model at all** is "under attack" — Felix muses whether you eventually "just give Claude memory and write ones and zeros." Stainless's "cyborg" / code-execution view (model writes code instead of calling many tools) is the same intuition (see [10](10-agents-as-users-and-infrastructure.md)).

## How to apply it

- Prefer **general tools** (bash, files, agentic search) and prune tools you don't need.
- Build **agent-native**: tools below features, features as prompts/skills, parity with the user.
- Keep logic in **skills/prompts**; reserve hard-coded tools for the genuinely repeatable, deterministic parts.
- Don't over-invest in **one tool or tier-list** — they invert in weeks; invest in your own practice.
- Budget for the **infrastructure wall**: sandboxing, long-running sessions, and lifecycle are the hard part.

## See also

- [09 · Choosing Models for Coding](09-choosing-models-for-coding.md) — the model side of the same decision
- [10 · Agents as Users & Infrastructure](10-agents-as-users-and-infrastructure.md) — MCP, code execution, harness+model pairing as a platform bet
- [06 · Compounding Engineering](06-compounding-engineering.md) — commands, skills, hooks as the compounding primitives
- [03 · Context Engineering](03-context-engineering.md) — agentic search and context-clean tooling

## Sources

Primary: *Inside Claude Code* (Boris Cherny / Cat Wu, 2025-10-29); *How Two Engineers Ship Like a Team of 15* (2025-06-11); *Vibe Check: Claude Cowork* (Felix, 2026-01-13); *The Secrets of Claude's Platform* (Angela / Caitlyn, 2026-05-08); *OpenAI's Codex This Model Is So Fast* (2026-02-18); *Building Is the Easy Part Now* (Mike Krieger, 2026-03-25); *Why Opus 4.5 Just Became the Most Influential AI Model* (Paul Ford, 2026-12-03); *Cognition's CEO on What Comes After Code* (Scott Wu, 2025-09-24); *Kevin Scott on the Future of Programming* (2025-05-21).
