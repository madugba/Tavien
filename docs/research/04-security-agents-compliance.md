# 04 - Security, Agent Runtime, Policy and Compliance

Author: Security and Compliance Architect / Agent-Runtime Engineer. Research date: 2026-11-10. All sources accessed 2026-11-10. This is engineering research, not legal advice. Items marked **[COUNSEL]** need a lawyer. Items marked **UNVERIFIED** could not be confirmed from a primary source.

## 1. Key findings (sourced)

### 1.1 MCP
- **The "2026-07-28" path is real and is the current revision.** The versioning page says "The current protocol version is 2026-07-28" and the previous final revision is 2025-11-25 (https://modelcontextprotocol.io/specification/versioning, https://modelcontextprotocol.io/specification/2026-07-28/changelog).
- **It is a breaking redesign.** The changelog removes the `initialize` handshake and protocol-level sessions (`Mcp-Session-Id`). Every request now carries version and capabilities in `_meta`. `server/discover` is mandatory for servers. SSE resumability is removed, so a broken stream means re-issuing the request. Server-initiated requests are replaced by multi round-trip requests (`input_required`). Tasks moved to an extension. Roots, Sampling and Logging are deprecated. HTTP+SSE transport is deprecated. Dynamic Client Registration is deprecated in favour of Client ID Metadata Documents (CIMD). Spec-wide statelessness means any server-side state uses explicit handles.
- **Authorization** (https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization):
  - It is optional and applies to HTTP transports only. STDIO "SHOULD NOT" use it and reads credentials from the environment.
  - The base is OAuth 2.1 (IETF draft-13). Servers MUST implement RFC 9728 Protected Resource Metadata.
  - Clients MUST send the RFC 8707 `resource` parameter in both authorization and token requests, and use PKCE.
  - Clients MUST validate `iss` (RFC 9207) before redeeming a code, and key client credentials by issuer.
  - Servers MUST validate the token audience. Servers MUST NOT accept or pass through other tokens.
  - Prefer CIMD over DCR. Use incremental scope step-up with a retry limit.
- **Python SDK:** the `mcp` package on PyPI is at **2.3.0** (2026-10-02). 2.0.0 shipped on 2026-07-28, the same day as the spec. 1.30.0 (2026-09-07) is still being released in parallel (PyPI JSON API). It is Tier 1 (https://modelcontextprotocol.io/docs/sdk). It needs Python 3.10 or newer per PyPI metadata. Whether v2.x implements every 2026-07-28 feature I did not verify (**UNVERIFIED**). Pin the version and run a conformance smoke test against 2–3 real servers.
- **Registry:** the official MCP Registry is in **preview** and "breaking changes or data resets may occur". It lists public servers only and is not intended to be consumed directly by host apps. Hosts should use downstream aggregators or a private registry (https://modelcontextprotocol.io/registry/about). It does no code scanning. So "curated directory" must be Tavien's own allowlist.
- **Attack classes and what the spec and docs actually say** (https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices):
  - Confused deputy through proxy servers.
  - Token passthrough.
  - SSRF during OAuth discovery. Block private, link-local and metadata IPs; pin DNS; validate redirects; use an egress proxy such as Smokescreen.
  - State-handle hijacking.
  - Local-server compromise. Show the exact command and require consent.
  - Malicious authorization URLs. Allow only http(s) and never shell-open a URL.
  - Mix-up attacks.
  - Localhost redirect impersonation.
  - Scope inflation.
- **Tool poisoning and rug-pull are not solved by the protocol.** Practitioner consensus (vendor and CSA material, so secondary) is to pin versions, hash tool definitions, alert on drift and log every call. MCP has no signed manifests, so clients re-fetch on every session (https://labs.cloudsecurityalliance.org/research-rb/csa-whitepaper-mcp-security-tool-poisoning-20260506-csa-styl/, https://zuplo.com/blog/remote-mcp-server-rug-pull/).
- **STDIO sandboxing:** Anthropic's `srt` (sandbox-runtime) wraps MCP server commands. It uses Seatbelt on macOS and bubblewrap on Linux. Network is deny-by-default through a filtering proxy. It is a **beta research preview**, and **Windows support is alpha** (needs an elevated install, Schannel/.NET TLS tools fail inside it) (https://github.com/anthropic-experimental/sandbox-runtime). Tavien's docs target Windows and macOS first, so this is not production-grade on Windows today.

### 1.2 Agent runtime (PyPI versions, 2026-11-10)
| Option | Fit for Tavien |
|---|---|
| **Pydantic AI 2.66.0** | Provider-agnostic: OpenAI, Anthropic, Gemini, Bedrock, Vertex, OpenRouter, Ollama, vLLM, LiteLLM and any OpenAI-compatible endpoint. Built-in `ModelFallback`. Durable execution via Temporal, DBOS or Prefect (co-maintained; one engine per agent). Typed, so tests can use fake models. (https://pydantic.dev/docs/ai/models/overview/, https://pydantic.dev/docs/ai/integrations/durable_execution/overview/) |
| LangGraph 1.2.20 | Postgres checkpointer (`PostgresSaver`) gives thread-level resume. Graph DSL and extra concepts. This is the current docs choice. (https://docs.langchain.com/oss/python/langgraph/durable-execution) |
| OpenAI Agents SDK 0.24.0 | Non-OpenAI models need Chat Completions or beta LiteLLM/any-llm adapters. Several Responses-only features are silently dropped. Tracing uploads to OpenAI by default. (https://openai.github.io/openai-agents-python/models/) |
| Claude Agent SDK 0.2.180 | It is "Claude Code as a library": it runs the Claude Code binary. It is Claude-only in practice, covered by Anthropic Commercial Terms, with branding limits. Wrong shape for a provider-agnostic core. (https://code.claude.com/docs/en/agent-sdk/overview) |
| Thin custom loop | Cheapest to understand, but you rebuild retries, durability and approval pause/resume. |

- **Browser tooling:**
  - **Playwright 1.65.0 + Playwright MCP.** Playwright MCP uses the accessibility-tree snapshot by default (no vision model needed). It states it "is not a security boundary". `--isolated` gives in-memory profiles. License Apache-2.0 (https://github.com/microsoft/playwright-mcp).
  - **browser-use 1.0.1.** MIT. 1.0 is a rewrite. Its README says to pin exact versions because releases break APIs. Its own docs say its domain restrictions and `sensitive_data` are "not a sandbox" (https://github.com/browser-use/browser-use).
  - **Browserbase.** Developer plan $20/mo with 100 browser-hours, then $0.12 per hour. Startup plan $99/mo with 500 hours, then $0.10 per hour. Proxies $10-12 per GB (https://www.browserbase.com/pricing).
  - **Browser Use Cloud.** The README lists browser time at $0.02 per hour, with model usage billed separately. I did not cross-check this on a pricing page.
- **Computer use (Anthropic):**
  - The toolset is a client toolset you run yourself. The SDK helper classes are beta.
  - Each screenshot costs about 1,000-1,800 input tokens.
  - Anthropic's own advice: run in a dedicated VM with minimal privileges, keep logins away from the model, allowlist domains, and have a human confirm consequential actions.
  - Prompt injection is called "a real risk" (https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use-tool).

### 1.3 Policy engines
- **Cedar:** CNCF Sandbox (accepted Oct 2025). I found no official Python binding. `cedarpy` 4.12.2 (2026-10-14) is community-maintained and its README says it is not AWS-supported (https://www.cncf.io/projects/cedar/).
- **OPA:** CNCF Graduated. Its creators and many Styra staff moved to Apple in Aug 2025, with governance and license unchanged (https://cloudnativenow.com/features/apple-buys-styra-brains-opa-remains-open/). In Python it means a sidecar or Wasm, plus Rego. The `opa-wasm` Python package was last released in 2022 (PyPI).

### 1.4 Law and compliance
- **EU AI Act Article 50 applies now.** The AI Omnibus (Regulation (EU) 2026/1744) was published 24 July 2026 and entered into force 27 July 2026.
  - It did **not** delay Art. 50(1) AI-interaction disclosure or the deployer duties, which "continue to apply from 2 August 2026".
  - The only relief is a grace period to **2 December 2026** for Art. 50(2) machine-readable marking, and only for systems **placed on the market before 2 August 2026**. A new product like Tavien gets no grace.
  - Annex III high-risk obligations moved to 2 December 2027.
  - (https://www.lewissilkin.com/insights/2026/07/27/the-digital-omnibus-on-ai-enters-into-force-today, https://www.gibsondunn.com/eu-ai-act-omnibus-agreement-postponed-high-risk-deadlines-and-other-key-changes/; one law-firm summary says 2026/1744 and 24/27 July. I could not open the Official Journal text.)
  - The final Code of Practice on marking/labelling AI-generated content was published 10 June 2026. The Commission's Art. 50 guidelines followed on 20 July 2026 (https://www.jonesday.com/en/insights/2026/06/european-commission-publishes-final-code-of-practice-on-marking-and-labelling-aigenerated-content, https://www.faegredrinker.com/en/insights/publications/2026/7/eu-ai-act-commission-confirms-transparency-code-of-practice-as-adequate-and-publishes-final-version-of-its-guidelines-on-transparency-obligations).
  - What Art. 50 says (https://artificialintelligenceact.eu/article/50/):
    - Users must be told they are interacting with AI, unless "obvious" to a reasonably well-informed person.
    - Providers must mark synthetic audio, image, video and text in machine-readable form.
    - Deployers must disclose deepfakes, meaning content resembling existing people that would falsely appear authentic.
    - Deployers must notify people exposed to emotion recognition.
    - Information is due "at the latest at the time of the first interaction".
  - A photoreal live avatar of a real person is the textbook deepfake and fails the "obvious" test.
- **Voice and biometrics (GDPR):** voice becomes Art. 9 biometric data when used to uniquely identify someone, for example speaker enrollment or diarisation by voiceprint (EDPB Guidelines 02/2021 on virtual voice assistants, https://www.edpb.europa.eu/system/files/2021-03/edpb_guidelines_022021_virtual_voice_assistants_adopted-public-consultation_en.pdf). I found no EDPB guidance that settles meeting transcription specifically, so treat voiceprints and face recognition as off by default **[COUNSEL]**.
- **US recording consent:** secondary sources list about 11-12 all-party states: CA, FL, IL, MD, MA, MT, NH, PA, WA, DE and NV (CT is civil only). They disagree on the count (https://getnextphone.com/blog/call-recording-laws-by-state, https://brasstranscripts.com/blog/state-by-state-recording-laws-2026-guide). Statutes were not checked **[COUNSEL]**. Illinois BIPA adds written consent for voiceprints.
- **Litigation signal:** *In re Otter.ai Privacy Litigation* (N.D. Cal., 5:25-cv-06911). Reportedly on 13 Aug 2026 the court let the core federal wiretap, CIPA and BIPA claims proceed. Single trade-press source; check the docket (https://www.uctoday.com/?p=748128, https://www.npr.org/2026/08/15/g-s1-83087/otter-ai-transcription-class-action-lawsuit).
- **Platform rules:**
  - Teams labels external bots "Unverified" and by default holds them in the lobby for organizer approval. Admins can block them (https://www.bleepingcomputer.com/news/security/microsoft-teams-now-lets-admins-block-external-bots-from-meetings, https://docs.recall.ai/docs/microsoft-teams-bots-faq).
  - Zoom's Linux Meeting SDK policy (quoted on its developer forum) says the Meeting SDK is for human use and does not support bots or AI notetakers. Bots should use RTMS, where the host gets an approval prompt and participants see a disclosure (https://devforum.zoom.us/t/question-on-linux-sdk-usage-policy/141545, https://devforum.zoom.us/t/understanding-zoom-rtms-and-third-party-app-permissions-in-external-meetings/131203).
  - Google Meet terms: not researched. This is for the meeting-platform specialist.
- **Data transfers:** the EU-US Data Privacy Framework stands, but Latombe's appeal (C-703/25 P) is pending at the CJEU. Plan for an EU-region option and SCC fallback (https://www.wilmerhale.com/en/insights/blogs/wilmerhale-privacy-and-cybersecurity-law/20251201-european-court-of-justice-to-review-challenge-to-eu-us-data-privacy-framework).
- **Likeness and voice cloning:** the federal NO FAKES Act (S.4591) cleared Senate Judiciary unanimously on 18 June 2026 but is **not law** (https://www.blackburn.senate.gov/2026/6/blackburn-coons-bipartisan-bill-to-protect-individuals-and-creators-from-deepfakes-passes-senate-judiciary-committee). It would not preempt existing state replica laws. State statutes (Tennessee, California, New York): **UNVERIFIED** in this pass **[COUNSEL]**. The Omnibus also adds a ban on "nudifier"-type systems (date unconfirmed).
- **SOC 2 (vendor-affiliated sources, treat as ranges):**
  - Type I in about 3-6 months with automation.
  - Platform licences from about $6-9k per year (Sprinto, Drata, Thoropass quotes cited) up to Vanta medians of about $17.7k per year at 100 staff.
  - Separate auditor fees of about $20-40k per audit. Thoropass bundles the audit.
  - Sources: https://blog.getagency.com/articles/soc-2-audit-cost-for-startups, https://soc2auditors.org/insights/soc-2-software-pricing-comparison/. Type II observation length: UNVERIFIED.

## 2. Numbers
| Item | Figure | Basis |
|---|---|---|
| Hosted browser, Browserbase | $0.10-0.12 per hour over plan; $20-99/mo plans | Official pricing |
| Hosted browser, Browser Use Cloud | $0.02 per hour (listed) | README |
| Self-hosted headless Chromium in a container | **UNVERIFIED**; assume well under $0.05 per hour at 2 vCPU. Measure it. | Assumption |
| Computer-use screenshot | about 1.0-1.8k input tokens each | Anthropic docs. Per-token price not verified here. A 30-step task is about 30-54k image tokens before context growth. |
| Playwright MCP (accessibility snapshots) | Text tokens only, no vision | Repo docs |
| Policy engine | $0 licence; Pydantic rules in-process (no sidecar) | Design choice |
| Audit log | Roughly 0.5-1 KB per event. 10M events per month is about 5-10 GB. Postgres cost, no extra service. | Assumption |
| SOC 2 Type I, year 1 | about $30-80k all-in for a small startup | Vendor blog estimate, low confidence |

## 3. Risks
1. **Indirect prompt injection** from web pages, meeting speech and screen content that triggers tool calls. Not fully solvable; limit the blast radius.
2. **Art. 50 exposure on day one.** The avatar needs on-screen and spoken disclosure plus machine-readable marking of generated video and audio. Penalties are reportedly up to 3% of turnover (ppc.land summary) **[COUNSEL]**.
3. **Emotion inference.** If the perception spec infers participants' emotions, Art. 50(3) notice applies. I recall the Act's Art. 5 also bans emotion recognition in the workplace; I did not verify this and flag it **[COUNSEL]**.
4. **Recording and wiretap class actions.** Otter shows the plaintiff playbook. Bots that join without host consent are the target.
5. **MCP spec churn.** The June-July 2026 redesign breaks 2025-era server and client code, and ecosystem servers lag.
6. **STDIO sandbox maturity.** `srt` is beta, and Windows is alpha.
7. **Hash-chained audit vs GDPR erasure.** Keep payloads outside the chain (crypto-shredding).
8. **Platform dependence.** Teams, Zoom and Meet can block or relabel bots at any time.

## 4. Recommendation
**MCP.**
- Target spec 2026-07-28 with the official `mcp` 2.x SDK (pinned). Keep 2025-11-25 compatibility only if a must-have server needs it.
- **MVP is remote Streamable HTTP only**, run server-side behind a Tavien MCP gateway. Use CIMD for client identity, with DCR as fallback. Send `resource` always and validate `iss`. Keep refresh tokens encrypted in a secrets vault, never exposed to the model.
- **Defer STDIO to phase 2.** Ship it only after a per-OS sandbox decision: Linux bubblewrap and macOS Seatbelt via `srt`, and a container or WSL2 on Windows. Until then, allow no user-defined commands.
- Gateway controls:
  - Allowlist of reviewed servers, with custom servers behind an admin toggle labelled unreviewed.
  - Hash every tool definition on discovery. On any change, quarantine the tool until re-approved.
  - Namespace tool names and strip or length-cap descriptions.
  - SSRF-safe fetcher: public IPs only, DNS pinned, redirects re-validated, through an egress proxy.
  - Per-tool grants and quotas.
  - Result size caps. Wrap tool output as quoted data, never as instructions.
  - Never forward the user's token to a second service (no token passthrough).

**Agent runtime: Pydantic AI + DBOS (durable steps in Postgres).**
- Why: it is the lightest fit for Python, provider-agnostic, typed, and testable with fake models. DBOS journals steps in plain Postgres, so there is no new service for a small team. Its approval and deferred-tool support suits the approval flow.
- **Verify before committing:** Pydantic AI's deferred tool approvals, and that DBOS can connect to your Postgres host (it needs a direct connection rather than a pooled one; this is **UNVERIFIED**, so spike it).
- Keep providers swappable:
  - All model calls go through one `ModelPort` module. Agents take a model-name string from config.
  - Use `ModelFallback` (for example Anthropic then OpenAI).
  - Optionally route through a gateway such as OpenRouter or LiteLLM.
  - Contract tests run the same suite against two providers.
- The real-time voice and avatar loop should **not** run inside an agent framework. Use a direct streaming pipeline and have it call the agent for slow tasks.
- **Runner-up: LangGraph** with `PostgresSaver`, if the owner prefers its graph model and ecosystem. Rejected: Claude Agent SDK (Claude-only, wraps a CLI binary), OpenAI Agents SDK (OpenAI-shaped).

**Browser.**
- Self-host Playwright in disposable, network-allowlisted containers, one per task, using the accessibility snapshot first. Add vision or computer-use only as a fallback (canvas apps, broken DOM). Computer use is worth it when the DOM is unusable and the task value exceeds the screenshot token cost.
- Use hosted browsers (Browserbase, about $0.10-0.12 per hour) only for sites that need stealth or proxies. Treat browser-use as a prototype tool until it stabilises.
- Never put credentials in the model context. Inject them at the browser layer.

**Policy and approvals.**
- Build a small **Pydantic policy module** with no external engine. It is a pure function: `decide(principal, action, resource, context) -> Allow | Deny | NeedsApproval(reason)`. Rules are versioned Python plus Postgres grants. Keep the request shape Cedar-like so you can swap in Cedar later.
- Autonomy levels L1-L4 map to default decisions per risk tier. Financial, legal, public-publish, destructive and admin actions always `NeedsApproval`.
- Approvals:
  - Suspend the durable workflow.
  - Bind the approval to the SHA-256 of the canonical arguments (one-time, expiring).
  - The approver must be a human principal, never the model or the MCP server.
  - Notify out of band.
  - Re-run policy on resume.
  - Support a kill switch.
- **Audit log:** one append-only Postgres table per tenant.
  - Writer role has INSERT only; UPDATE and DELETE are revoked and a trigger raises on both.
  - Each row stores `hash = sha256(prev_hash || canonical_json(event))` with `prev_hash`.
  - Anchor the daily head hash to object storage with Object Lock (or a signed timestamp).
  - Store redacted metadata and payload hashes in the chain, and keep encrypted payloads elsewhere with TTL and per-tenant keys so you can erase them.
  - Monthly partitions.
  - Supabase can host this, with RLS and a dedicated role. Verification is a nightly job.

**Compliance.**
- Disclose by default: the avatar shows a persistent "AI" label, announces itself by voice on join, and posts a chat notice. Meeting-level consent follows the strictest regime (all-party). Join only after host approval, and respect Teams "Unverified" and Zoom RTMS prompts.
- Mark generated audio and video machine-readably from day one (C2PA or watermark per the Code of Practice **[COUNSEL]**).
- No voiceprints, no face recognition and no emotion inference in MVP. Only the verified user's own likeness and voice, with liveness-verified, recorded, revocable consent. No third-party or celebrity likenesses.
- Do a DPIA anyway (large-scale, novel tech, meeting audio and video) **[COUNSEL]**. Default to EU region hosting for EU tenants.
- SOC 2: do not buy a platform for the MVP. Build SOC 2-aligned controls now, keep evidence in the repo, request quotes from 2-3 vendors when the first enterprise deal asks, target Type I at about 6-9 months, then Type II. Check that Supabase and Vercel have current reports and inherit them. Do not claim compliance before an audit.

### Security baseline: 15 controls in rank order
1. Policy enforcement outside the model (deny by default, per-tool grants).
2. Human approval for high-risk actions, bound to the argument hash.
3. AI disclosure plus meeting consent flow (Art. 50 and recording laws).
4. Tenant isolation (Postgres RLS, tested) and per-tenant keys.
5. Secrets vault, with OAuth tokens never visible to the model, logs or URLs.
6. MCP gateway: allowlist, tool-definition hashing and drift quarantine, no token passthrough, audience and `iss` validation.
7. Untrusted-content handling: tool and web output as quoted data, size caps, no policy or system-prompt edits from content.
8. Egress control and SSRF-safe fetcher for browser workers and MCP.
9. Disposable sandboxed browser containers with no credentials inside.
10. Append-only hash-chained audit log.
11. Budget, step, time and rate limits per task and tenant, plus a kill switch.
12. Encryption in transit and at rest, backups with a tested restore.
13. Data retention and deletion (media off by default, crypto-shredding).
14. Machine-readable marking of generated media, plus consent records for likeness and voice.
15. Dependency and supply-chain hygiene (pinned versions, lockfile, SCA, secret scanning in CI).

**Can wait:** STDIO MCP, a public MCP directory, OPA or Cedar, SOC 2 platform and audit, SSO/SCIM, customer-managed keys, third-party pen test (do before the first enterprise deal), SIEM.

## 5. Where the initial docs are wrong, outdated or optimistic
- Docs say "use the current Streamable HTTP and protocol version negotiation". The 2026-07-28 revision removed negotiation by handshake, sessions and SSE resumability. The docs' MCP gateway design must be rewritten around stateless requests and `server/discover`.
- Docs mention OAuth and tokens generally. They omit CIMD (DCR is now deprecated), `iss` validation and the `resource` parameter.
- Docs plan STDIO supervision from the Qt desktop companion in the MVP. The sandbox tooling is beta (Windows alpha), so defer it.
- Docs treat EU AI Act review as pending future work. Article 50 has applied since 2 August 2026, and Tavien's avatar and voice trigger it immediately. A schedule that treats this as "later" is wrong.
- Docs list LangGraph as the orchestrator. It works, but Pydantic AI + DBOS is lighter for this team and rule.
- Docs do not state that Zoom's Meeting SDK policy excludes bots (RTMS is the path) or that Teams flags bots as "Unverified".
- Audit and approval are listed but not designed. The designs above are the concrete version.

## 6. Open questions for the owner
1. Is the avatar ever a **real person's likeness** (the user's) or only stock or synthetic avatars? This drives Art. 50(4), likeness consent and counsel scope.
2. Will you sell to EU customers at launch? If so, is an EU-region deployment in scope?
3. Budget and appetite for a privacy and AI-Act counsel review before launch.
4. Which legal entity and jurisdiction will be controller or processor, and who signs DPAs?
5. Is there an enterprise customer with a SOC 2 deadline, and will they accept "in progress"?
6. Are meeting recordings and transcripts retained by default, or off unless the host enables them?
7. Approval of the rule that custom (unreviewed) MCP servers are admin-only in MVP.
