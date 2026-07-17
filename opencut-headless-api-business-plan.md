# Business Plan: Headless Video Rendering API (Built on OpenCut)

*Expands idea #2 from `opencut-business-ideas.md`. Drafted July 2026.*

## One-Line Pitch

"Send us JSON, get back a finished video." A cloud API that renders videos programmatically from templates — for developers, no-code users, and businesses that need videos generated automatically at scale — built on OpenCut's open-source engine instead of a proprietary one.

---

## 1. The Business Model (How Money Is Made)

**Usage-based SaaS.** Customers pay a monthly subscription that includes a bucket of render minutes; heavy users pay overage per minute. This is the proven model in this exact market (Shotstack, Creatomate, JSON2Video all price this way).

Why this model works well here:

- **Costs scale with revenue.** Your main cost is compute (servers rendering video). A customer who renders 10,000 minutes pays you more AND costs you more — margins stay healthy at every size.
- **Revenue is recurring and grows on its own.** Customers wire your API into their product or workflow. Once embedded, they don't churn easily (switching means re-engineering), and as *their* business grows, their render volume — and your revenue — grows without you selling anything.
- **You charge for convenience, not code.** OpenCut being free doesn't undercut you; nobody wants to run render servers, manage queues, handle failures, and store output. That operational pain is the product.

**Revenue streams, in order of when they arrive:**

1. **Self-serve subscriptions** (month 1 of launch) — developers and no-code users, $0–500/mo each
2. **Overage / high-volume usage** (as customers grow) — the best revenue: zero sales effort
3. **Enterprise self-hosted** (year 1–2) — the renderer runs in *their* cloud for compliance; annual license + support contract, $10k–50k/yr. **This is the offer proprietary competitors structurally cannot match**, because their engine is closed. Yours is open — you're selling the orchestration layer, support, and updates.
4. **Template marketplace** (later) — designers sell render-ready templates, you take 20–30%

---

## 2. Market & Competition

**Who buys this today (proven demand, these companies exist and charge for it):**

| Use case | Example customer | What they render |
|---|---|---|
| Personalized sales video | Outreach/sales tools | "Hi {name}" videos per prospect |
| E-commerce | DTC brands, agencies | Product video per catalog item |
| Real estate | Listing platforms, agents | Walkthrough video per MLS listing |
| Ad creative testing | Performance marketers | 50 hook/CTA variants per campaign |
| Social automation | Faceless channels, news/sports sites | Daily auto-generated clips |
| SaaS "export as video" | Fitness, education, analytics apps | Year-in-review, workout recaps, report videos |

**Competitors and their pricing (validates willingness to pay):**

- **Shotstack** — $0.40/min pay-as-you-go, ~$0.20/min on subscription, plans from ~$49/mo
- **Creatomate** — credit plans roughly $41–299/mo; ~14 credits per 720p minute
- **JSON2Video, Plainly, Remotion (Lambda)** — same market, similar pricing

The market supports multiple seven-figure-revenue players. You don't need to kill them — you need a differentiated slice.

**Your differentiation (why pick you over Shotstack):**

1. **Templates are real OpenCut projects.** Competitors force you to design templates in their proprietary web builder. With you, anyone designs a template in the full, free OpenCut editor — every OpenCut user is a potential template author. Design visually, render programmatically.
2. **No lock-in.** Templates and the engine are open. Enterprises hate rendering pipelines they can't take with them.
3. **Self-hosted option.** Healthcare, finance, government, and EU-data-residency customers can't send footage to a US SaaS. Open engine makes "run it in your VPC" a real product tier.
4. **Price.** Open core + modern Rust renderer should let you undercut $0.20/min meaningfully.

---

## 3. Go-To-Market (How Customers Find You)

Developer-led, content-driven — no sales team needed until enterprise deals appear:

1. **Ride OpenCut's community.** 66k+ GitHub stars, active Discord. Be visibly helpful there; when headless mode ships, be the "easiest way to use it in the cloud" on day one. This audience is free and pre-qualified.
2. **No-code connectors early.** Zapier, Make, and n8n integrations ("new spreadsheet row → rendered video") open the market to non-developers, who outnumber developers 100:1 and churn less.
3. **Programmatic SEO with the product itself.** Publish template galleries and tutorials: "auto-generate real estate videos from a spreadsheet," "make 100 product videos from a Shopify export." Each is a landing page that demos the API.
4. **Free tier with watermark.** Standard in this market; the watermark is itself advertising.

---

## 4. Pricing Sketch

| Tier | Price | Included | Target user |
|---|---|---|---|
| Free | $0 | 30 min/mo, watermarked, 720p | Tinkerers, evaluation |
| Starter | $39/mo | 300 min, 1080p | Indie devs, small automations |
| Growth | $149/mo | 1,500 min, priority queue | Agencies, small SaaS |
| Scale | $499/mo | 6,000 min, 4K, SLA | Production SaaS integrations |
| Enterprise | $10k–50k/yr | Self-hosted / VPC, support, SSO | Compliance-bound orgs |

Overage ~$0.08–0.15/min (undercuts Shotstack's $0.20). Rendering compute on spot/serverless instances should land around $0.01–0.04 per output minute, giving 60–80% gross margins.

---

## 5. Build Plan (Phased)

**Phase 0 — Validate before building much (now, ~1 month):**
OpenCut's headless mode is roadmap, not shipped. Don't wait and don't bet on their timeline. Prototype the rendering pipeline with what exists today (OpenCut classic's engine, or ffmpeg-based glue behind the same API design). More importantly: get 10 conversations with people in the use-case table above. Pre-sell if possible.

**Phase 1 — MVP (2–3 months):**
- One endpoint: `POST /render` with template + JSON data → video URL via webhook
- Template = OpenCut project file with variable placeholders ({{name}}, {{image_url}})
- Queue + workers on cloud instances, S3 storage, usage metering, Stripe billing
- 10–20 polished starter templates (this matters more than API features)

**Phase 2 — Launch (months 3–6):**
- Free tier, docs, Zapier/Make/n8n connectors, template gallery site
- Launch on Product Hunt / Hacker News / OpenCut Discord
- Target: 50 paying customers ≈ $3–5k MRR

**Phase 3 — Deepen the moat (months 6–18):**
- Adopt OpenCut's official headless engine when it ships (your API contract stays stable — customers don't notice the engine swap)
- Template marketplace; enterprise self-hosted tier; first $10k+ annual deals
- Target: $20–40k MRR, decide whether to raise/hire or stay solo

**Startup costs are low:** this is bootstrappable. Real costs are your time, ~$200–500/mo infrastructure until volume grows (rendering costs track revenue), and maybe a freelance designer for templates.

---

## 6. Risks & Honest Answers

- **OpenCut headless slips or changes.** *Mitigation:* Phase 0/1 doesn't depend on it — your product is the API contract, queueing, billing, and templates; the engine behind it is swappable.
- **OpenCut team launches an official cloud API.** Real risk given fal.ai backing. *Mitigation:* speed (be there first), no-code distribution, verticals and enterprise/self-hosted — the segments an official offering serves last. Worst case, you're the obvious acquisition.
- **Incumbents are established.** Shotstack/Creatomate have years of head start. *Mitigation:* you're not out-featuring them, you're out-positioning them — open engine, editor-designed templates, self-hosting. Compete on the axis they can't move to.
- **It's a developer product and you need distribution.** The no-code connectors and template SEO exist precisely to avoid depending solely on developer adoption.

## 7. What "Success" Looks Like

- **12 months:** 100–200 paying customers, ~$15–30k MRR, 2–3 enterprise conversations
- **24 months:** $50k+ MRR, marketplace live, self-hosted tier sold, sustainable solo/2-person business
- **Ceiling:** Shotstack-class businesses in this market reach seven figures ARR; the embed/enterprise angle is what could push past that.
