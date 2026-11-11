# 01 - Market and Product Research

Author: product owner / market analyst. Date: 2026-11-10. All sources accessed 2026-11-10 (URLs in the Sources list). "3P" means a third-party aggregator or blog, not the vendor's own page. "UNVERIFIED" means I could not confirm it. Costs are hypotheses unless marked verified. Nothing here is legal advice.

## 1. Key findings (sourced)

**1.1 Name collision (risk flag, read first)**
- **tavien.ai is live and in the same category.** It is titled "Tavien - Ask your business anything": a private AI assistant for executives, connected to ERP, CRM, accounting, email and documents, with approvals and an audit log. RDAP shows it was registered 2026-09-21 and expires 2028-09-21. It is hosted on GitHub Pages with a Formspree contact form, and I found no company name on the page. It may be the owner's own page. If it is not, it is a direct conflict. See Open Questions.
- **tavien.com** is a parked "for sale" page. RDAP shows registration in 2006 and expiry 2027-01-30. **tavien.io** returned an HTTP 525 error. My proxy blocked .app, .de, .eu and .co, so those are UNVERIFIED.
- **Tavus** (tavus.io) is the closest confusable mark. It makes real-time "AI humans" and raised a $40M Series B in Nov 2025 led by CRV (3P press coverage). Tavus Inc. holds about 9 US marks, mostly Class 42 (3P trademarkdir). Tavien and Tavus differ by roughly one syllable and sit in the same market, so the risk is highest if the avatar stays the core story.
- Other near names in Class 42 AI/software on aggregators: TAVRI, TAVLO (AI business communications), TAVA AI, TAVIO, TAVILY (AI search API, widely used by agent builders). I found no mark or company named exactly "Tavien" in web search. This is **not** a registry search. USPTO, EUIPO/TMview, WIPO and DPMA were not checked directly, so I cannot call the name clear.

**1.2 Meeting AI is crowded and being commoditized**
- Native notes are now bundled. Zoom has ZoomMate (free tier: 3 summaries a month; paid add-on). Google "Take notes for me" launched 2026-06-29 on AI Pro/Ultra and eligible Workspace plans, for meetings the user hosts. Microsoft Copilot Business is $18/user/month (annual promo; list $21).
- Demand is real. Otter reported $100M ARR (company press release, Mar 2025) and launched a voice Meeting Agent that speaks in calls. Granola raised $125M at a $1.5B valuation (Mar 2026, Bloomberg/TechCrunch). Fireflies reached a $1B valuation (3P).
- Platforms are restricting external bots. Teams has an `ExternalBotAccessMode` policy. The default is lobby approval for detected bots, an admin can set `BlockDetectedBots`, and auto-block rolled out from Aug 2026. Copilot and Entra-registered bots are exempt (UC Today, citing MC1251206). This favors **tenant-registered apps** and **bot-free capture**.

**1.3 Live avatars are not unique and cost real money**
- HeyGen avatars join Zoom (announced Oct 2024). Pika's PikaStream joins Google Meet (Apr 2026). TruGen AI and 1mind ship similar products, and Zoom shipped avatars in Mar 2026 (all 3P news). Real-time vendors price per minute (table below).
- I found no solid evidence that avatars raise trust over voice. Surveys and the uncanny-valley literature are mixed or weak.

**1.4 Legal climate for meeting AI is hostile**
- Otter faces a class action (Brewer, N.D. Cal., filed 2025-08-15) over recording without participant consent. Fireflies faces two BIPA voiceprint suits (Cruz, Dec 2025; Fricker, 2026). A May 2026 motion-to-dismiss hearing is reported by secondary sources, and I found no ruling.
- EU AI Act Art. 50 transparency duties (telling people they are talking to AI) have applied since 2026-08-02 (law-firm alerts). Marking of synthetic content may be delayed to 2026-12-02 (sources conflict).
- Germany: § 201 StGB turns on whether audio is stored, and works-council approval is typically required (§ 87(1) no. 6 BetrVG) (3P law-firm and vendor blogs). US sources disagree on the all-party-consent states (9 to 12 depending on the list). A visible bot is not accepted as consent.

## 2. Numbers (competitors, USD unless stated)

| Player | Product | Price (source) | Strength | Gap for Tavien |
|---|---|---|---|---|
| Otter | Notes + voice agent | Pro $16.99/mo ($8.33 annual); Business $30 ($19.99 annual); 1,200 min Pro (otter.ai, official) | Scale, brand, CRM sync | Lawsuit and consent exposure; notes-centric |
| Fireflies | Notes | Pro ~$18 monthly, ~$10 annual; Business ~$29/$19 (3P; the official page showed no numbers) | Integrations | Same bot and BIPA exposure |
| Fathom | Notes | Free; Premium $20; Team $19; Business $34 (fathom.ai, official) | Free tier is generous | Price anchor: free |
| Granola | Bot-free notes | Free; Business $14; Enterprise $35 (official) | Bot-free UX; MCP, API | No actions or voice |
| Read.ai | Notes + analytics | Free; Pro $15 annual / $19.75 monthly (official) | Analytics | Same as above |
| Zoom ZoomMate | Native | Free tier limited; paid "starting at" price not shown on the page. 3P says $20/user, 2,200 credits | Bundled | Zoom-only |
| MS Copilot | Native | Copilot Business $18 promo / $21 list yearly, $25.20 monthly (microsoft.com) | Tenant trust | M365-only; heavy |
| Gemini in Meet | Native | Included in AI Pro/Ultra and eligible Workspace (blog.google) | Bundled | Google-only; host-only |
| Tavus | Dev API + PALs | Free 20 min; Starter $22/60 min; Builder $59/175 min, then $0.35/min; Growth $397/1,300 min, then $0.31; Business $975/4,000 min, then $0.26. PALs consumer: $20/150 min, $50/500 min (official) | Funded avatar models | No agent or tools depth; name risk |
| Anam | Dev API | Starter $12/50 min; Growth $299/2,000 min, then $0.12; Pro $999/8,000 min, then $0.11; Enterprise from $0.04/min (official) | Cheap, low latency | Avatar-only |
| Simli | Lip-sync only | Hobby $10/1,000 min ($0.01/min); Scale $249/27,500 min (official) | Very cheap rendering | Rendering only |
| HeyGen / Synthesia | Video + interactive | HeyGen Creator $29, Pro from $49, Business $149 plus $20/seat; Synthesia Starter $19, Pro $89. Live-avatar API price UNVERIFIED (not on the pages) | Brand and content tooling | Not agent platforms |
| D-ID | Avatar API | UNVERIFIED (page returned no pricing) | - | - |
| ChatGPT | Agent mode | Plus $20; Pro $100 or $200 (3P; the official page returned 403, so agent limits are UNVERIFIED) | Distribution | No meeting presence |
| Claude | Assistant | Pro $17 annual / $20 monthly; Max $100; Team $20-25 standard, $100-125 premium (claude.com, official) | Model quality | No meeting presence |
| Lindy | Agent | Team $29.99/user, 3,000 pooled credits; $10 per 1,000 credits; meeting notes, MCP, 1,500+ integrations (official) | Closest "do the work" rival | Weak voice and avatar |
| Dust | Enterprise agents | Pro EUR24 annual / EUR30 monthly; Max EUR120 / EUR150 (official) | Governance | Not meeting-first |
| Manus | Agent | About $20 / $40 / $200 (3P); ownership saga conflicting | Autonomy | Consumer-only |
| Genspark | Agent | UNVERIFIED (page blocked) | - | - |
| Recall.ai | Bot infrastructure | $0.50/hr bot; transcription $0.15/hr; GPU bots (avatar) +$1/hr; Zoom, Meet, Teams, Webex (official) | Buy instead of build | A dependency |
| Zoom RTMS | Inbound media | About $0.01/min, community-reported only (UNVERIFIED) | Bot-free inbound | Cannot speak back |

Unit-cost anchors (hypotheses; the realtime researcher owns exact figures):
- Meeting notes only: about $0.01-0.02 per minute (bot $0.0083/min + transcription $0.0025/min + LLM summary).
- Voice agent: $0.05 to $0.45 per minute depending on caching (3P OpenAI Realtime math).
- Avatar render: $0.01/min (Simli) up to $0.11-0.35/min retail at all-in vendors.

## 3. Positioning and scope

**Verdict: the documented scope is not a viable launch scope.** It combines a universal agent, avatar, meeting participant, MCP platform, browser and desktop automation, and a developer ecosystem for four customer groups. Each piece has a funded specialist (Lindy/Dust, Tavus/Anam, Otter/Zoom, Recall) or is already table stakes: MCP is listed by Otter, Granola, Lindy and Dust. A small team would ship five mediocre products.

**Proposed wedge (hypothesis, to be validated):** *a consent-first AI teammate that finishes the work after client calls.*
- **ICP:** owner-operators and ops leads at 5-50 person B2B service firms (agencies, consultancies, recruiters), EU/DACH first. They hold many client calls, have no ops staff, and care about GDPR and client optics.
- **Job to be done:** "After every client call, the follow-up email, CRM update, tasks and proposal draft exist and are approved by me, with proof of what was done." This is action with approval and audit, not notes.
- **Why now:** Teams auto-blocks external bots and tenant-registered apps are exempt. Consent lawsuits make uninvited bots a client-trust problem. Art. 50 makes disclosed AI participation the legal default. MCP makes integrations cheap. Notes are commoditized, so only post-call action and trust can still differentiate.
- **Honest limit:** Lindy, Zoom ZoomMate (agentic tasks) and Otter (workflows) are moving toward the same job. The defensible parts are consent and audit UX, EU residency, and low cost. Test demand before building.

**Phasing**

| Phase | In | Out (defer) |
|---|---|---|
| MVP | Web app and auth. Audio-only voice session. Meeting capture by upload, own-session recording, or one bought bot path. Summary, then proposed actions, then approve, then execute through 3 to 5 integrations (calendar, email draft, CRM or Notion/Linear) via remote MCP. Per-tool permissions, audit log, consent banner, billing, waitlist. | Avatar, desktop companion, browser and desktop automation, STDIO MCP, marketplace, Business/Platform tiers |
| v1 | Bot join for Zoom, Meet and Teams (buy Recall rather than build), disclosed, with a Teams tenant-registered app path. Speak-on-invite in audio only. Team workspace and roles. Memory. Optional stylized avatar in Tavien's own session via a cheap renderer. Bring-your-own remote MCP. | Photoreal avatar |
| Platform | SDK and API, curated directory, desktop companion, browser automation, photoreal or custom avatars, enterprise governance. | - |

**Cut now:** the Consumers and Developers segments, desktop control, and the "universal agent" claim in marketing. The avatar becomes an optional skin, not the core.
**Runner-up wedge:** AI practice partner (sales roleplay, interviews) with an avatar. It has lower platform risk because it runs in Tavien's own session, but Synthesia, Tavus and HeyGen already sell roleplay, and per-minute avatar cost squeezes margin.

## 4. Pricing and unit economics (hypotheses)

| Plan | Price (anchors) | Includes (hypothesis) |
|---|---|---|
| Free | $0 (Fathom and Granola both have free tiers) | 3 meetings/mo, 10 voice min, 20 task credits; no avatar |
| Pro | $20-24/user/mo (Claude $20, ChatGPT $20, Fathom $20, Otter $17, Copilot $18-21, Zoom paid about $20 per 3P) | 10 meetings, 30 voice min, 100 task credits |
| Team | $30-40/seat, 3-seat minimum (Lindy $29.99, Dust EUR24-30, Fireflies Business $19-29, Fathom Business $25-34, Granola Enterprise $35) | Shared workspace, roles, audit export, EU residency, overage packs |
| Add-ons | Metered avatar and extra voice minutes at roughly 2-3x COGS | Avatar minutes are not bundled |

Willingness-to-pay evidence: the $20/month price point is firmly established across assistants and notetakers. Otter reached $100M ARR with Pro and Business seats. Granola's valuation rose from $250M to $1.5B in about a year. Zoom reportedly raised its AI add-on from $12 to $20 (3P pricingsaas, UNVERIFIED). Counter-evidence: free tiers are generous and native bundles push the price of plain notes toward zero. **Plain notes are not a revenue line.** Revenue has to come from actions, governance and voice.

Margin sketch for one Pro user (all hypotheses): 8 meetings x 30 min notes = about $3.6; 30 voice min at $0.07-0.15 = $2-4.5; 100 agent tasks at $0.03-0.05 = $3-5; infra and payment fees about $1.5. COGS is about $10-15 against $20-24 revenue, which is only 40-55% gross margin at full usage. Hence caps and credits. **Target: at least 65% blended gross margin, at least 55% on voice-heavy users, and no free-tier avatar.** Bundling avatar minutes at Tavus overage rates ($0.26-0.35/min) would lose money at $20/month, so avatar stays metered.

## 5. Go-to-market and validation

First three channels:
1. **Founder-led design-partner outreach.** Target 15-20 interviews with agency and consultancy owners via LinkedIn and EU founder communities. Offer concierge onboarding for free.
2. **Intent content and comparison pages.** Examples: "bot-free, GDPR-safe AI follow-up", "Teams blocking AI bots: what to do", "Otter/Fireflies alternative". The lawsuits and Teams policy give timely hooks.
3. **Launch and directories.** Product Hunt, Hacker News, and listings in MCP, Notion, HubSpot and Zapier directories.

Pre-build validation (thresholds are my proposals, not benchmarks):
- Landing page and waitlist with a clear price on it: at least 5% visitor-to-waitlist on targeted traffic.
- At least 15 interviews. At least 5 design partners using it weekly (3+ meetings a week). At least 30% willing to pre-pay or sign an LOI at $25 or more.
- Once built: share of meetings with at least one approved action, actions approved per meeting, day-30 retention, COGS per meeting, participant opt-out or complaint rate, and time to first approved action.

Launch checklist: name and trademark clearance with counsel; domains; privacy policy, DPA and subprocessor list; consent and disclosure UX; Stripe billing; analytics; status page; demo video; support inbox; a published retention and no-training-on-customer-data policy.

## 6. Risks

| Risk | Severity | Mitigation |
|---|---|---|
| Name collision (Tavus; tavien.ai unresolved) | High until cleared | Counsel search now; fallback names; secure domains |
| Platform hostility to bots (Teams auto-block; Meet and Zoom host controls) | High | Tenant-registered app path, bot-free capture, host-initiated join |
| Consent and wiretap or BIPA litigation | High | Consent-first UX, no voiceprints or speaker biometrics by default, no training on customer audio, retention limits, EU residency |
| Big-tech bundling (Zoom, Copilot, Gemini) | High | Do not sell notes; sell cross-platform actions and governance |
| Avatar uncanny valley and cost | Medium | Voice-first; stylized avatar optional; metered |
| Voice and avatar COGS crush margin | Medium | Caps, credits, caching, audio-only default |

## 7. Recommendation

**Launch as a consent-first post-meeting "do the work" agent with audio-only voice, three to five integrations, and approvals plus audit. Buy bot infrastructure; do not build it. Treat the avatar as a later optional skin.** Rationale: it is the only slice where a small team can reach production quality, the cost per meeting is low, and it matches the owner's priorities of cost efficiency and durability. Runner-up: avatar-based practice and roleplay for sales and interview training, in Tavien's own session.

## 8. Where the initial docs are wrong, outdated or optimistic
- Scope: four customer groups and a three-layer product (Personal, Business, Platform) for a team of agents is not credible. The docs contain **no pricing, unit economics or demand evidence**; the roadmap has only "business model directions".
- "Real-time avatar is the core differentiator": HeyGen, Pika, TruGen, 1mind and Zoom already ship it, and the trust benefit is unproven.
- "MCP extensibility" and "extension directory" are presented as differentiators. MCP is now a checkbox feature.
- Meeting-provider risks are understated: Teams auto-block policy and the lawsuits are not covered in the docs.
- The docs list the name as "clearance pending" and do not mention tavien.ai or Tavus.

## 9. Open questions for the owner
1. Do you own tavien.ai (registered 2026-09-21)? If not, who does? If it is yours, is "private executive assistant over ERP/CRM" the intended direction? That wedge is sharper than the docs.
2. Budget for trademark counsel (EUIPO/DPMA/USPTO search) and your appetite to rename if Tavus risk is confirmed.
3. Target geography and legal entity (EU/DACH first?), and who signs off the privacy, DPA and consent policy.
4. Can you open intros to 15-20 service-firm owners for design-partner interviews?
5. Monthly budget ceiling for free-tier and pilot compute (voice minutes).

## Sources (accessed 2026-11-10)
Official: tavus.io/pricing; otter.ai/pricing; fireflies.ai/pricing; granola.ai/pricing; fathom.ai/pricing; read.ai/pricing; heygen.com/pricing; synthesia.io/pricing; anam.ai/pricing; simli.com/pricing; recall.ai/pricing; claude.com/pricing; lindy.ai/pricing; dust.tt/home/pricing; microsoft.com/en-us/microsoft-365-copilot/pricing; zoom.com/en/products/ai-assistant; blog.google/products-and-platforms/products/workspace/take-notes-for-me; tavien.ai; tavien.com; rdap.verisign.com/com/v1/domain/tavien.com; rdap.org/domain/tavien.ai.
Secondary: uctoday.com/microsoft-teams-to-block-external-bots-automatically-as-ai-notetaker-crackdown-hardens; uctoday.com/ai-meeting-bots-controls-microsoft-zoom-google; uctoday.com/heygen-reveals-ai-avatars-that-can-attend-zoom-meetings-for-you; businesswire.com/news/home/20250325249430/en (Otter ARR); businesswire.com/news/home/20251111507298/en (Tavus Series B); bloomberg.com/news/articles/2026-03-25/ai-notetaker-granola-hits-1-5-billion-value-in-125-million-funding; npr.org/2025/08/15/g-s1-83087/otter-ai-transcription-class-action-lawsuit; workplaceprivacyreport.com (Fireflies BIPA); goodwinlaw.com 2026/08 EU AI Act alert; lutzabel.com (§ 201 StGB); forasoft.com/blog/article/openai-realtime-api-pricing; devforum.zoom.us (RTMS pricing); trademarkdir.com and markinton.com (aggregator trademark listings); pricingsaas.com/companies/zoom (Zoom price change); alternativeto.net (Pika); emelia.io/hub/ai-teammates-enterprise (TruGen, 1mind).
