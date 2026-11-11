# Tavieri - Platform Design Spec (v1)

Status: **proposed, awaiting owner review.** Evidence: `docs/research/01..05` (sources dated 2026-11-10).
The original idea docs are superseded where this spec disagrees; reasons are given inline.

## 1. Understanding (what the owner asked for)
Tavieri is a highly intelligent, model-agnostic agent platform that can do almost any digital work through
MCP tools, voice, vision and browser use, with coding added later. The team owns delivery end to end.
Priorities: cost efficiency, durability, scalability, clean code (<= 100 lines per file), best practice.
Fable is banned; Sonnet 5.5 is the default model; Opus 5.5 is for hard cases only.

## 2. Key conclusion: one general core, a narrow launch wedge
The architecture stays general (capability registry, MCP, policy, model port, memory) so nothing needs
rewriting to reach "does everything". What ships first is chosen by evidence, not ambition:
the draft's full scope would produce five mediocre products against five funded specialists (report 01).

**Launch wedge (hypothesis to validate):** a consent-first AI teammate that finishes the work after client
calls for 5-50 person B2B service firms, EU/DACH first. Flow: capture -> summary -> proposed actions ->
human approval -> execution through MCP integrations -> audit proof. Notes alone are commoditised
(Zoom/Teams/Meet bundles); approved action, consent UX, EU residency and low cost are the differentiators.
Runner-up wedge: avatar practice partner (sales/interview), rejected for launch because per-minute avatar
cost squeezes margin and Tavus/HeyGen/Synthesia already sell it.

## 3. Decisions (each becomes an ADR in the first sprint)
| # | Decision | Rationale | Runner-up / switch trigger |
|---|---|---|---|
| D1 | Python modular monolith (FastAPI, Pydantic, asyncio); Next.js/TypeScript web | Owner constraint; one language for agents + API | - |
| D2 | **Supabase Pro (EU) as data layer only**: Postgres, pgvector, Auth, Storage. Not for media or workers | Cheapest managed Postgres+Auth; plain-Postgres exit path; own `Identity` port limits lock-in | Hetzner + Coolify when spend > ~$3-5k/mo |
| D3 | **Fly.io** (Frankfurt), session-per-Machine for long jobs; **Vercel** for web | Cloud Run's 60-min WebSocket cap breaks hour-long sessions | Hetzner; Cloudflare for web |
| D4 | **DBOS** durable workflows on Postgres; no Redis, no Temporal at launch | One datastore, durable retries/approvals, cheapest | Temporal if scale demands |
| D5 | Agent runtime **Pydantic AI** behind our own ports | Light, typed, provider-agnostic, testable | LangGraph + PostgresSaver |
| D6 | Own thin **`ModelProvider` port**; native Anthropic + OpenAI-compatible adapters; OpenRouter/Vercel gateway only as optional adapter; **no LiteLLM in core** (malicious releases, Mar 2026) | Keeps caching/thinking/tool-search; metering and caps in our ledger | - |
| D7 | Voice/real-time on **LiveKit Agents** (Python), cascaded STT+LLM+TTS; avatar via adapter (**Simli** first, ~$0.01/min); no self-hosted GPU avatar | Cheapest verified path; avatar vendor swings cost 10x | Pipecat |
| D8 | **Buy the meeting layer: Recall.ai Output Media** (two-way audio + webpage-as-camera on Zoom/Meet/Teams, ~$0.50-0.60/h) | The draft's "Meet/Teams infeasible" is wrong; the real gate is platform bot policy | Attendee (licence review), MeetingBaaS |
| D9 | **MCP**: spec 2026-07-28, official `mcp` 2.x SDK, remote Streamable HTTP behind our gateway; STDIO deferred | 2026-07-28 is stateless and breaking; STDIO sandboxing is still beta | - |
| D10 | Custom Pydantic **policy engine** (`decide() -> Allow/Deny/NeedsApproval`), hash-bound approvals, append-only hash-chained audit in Postgres | Simplest that scales; Cedar-shaped for later swap | OPA/Cedar |
| D11 | Models: user tiers **Fast / Balanced (default) / Max / Auto**, plus pin-a-model and BYOK; routing is **eval-gated**; Fable excluded from the product too | Meets "configurable + smart + cheap" | - |

## 4. Architecture
Packages under `packages/py/`: `kernel`, `identity`, `tasks`, `tools` (capability registry), `mcp` (gateway),
`policy`, `memory`, `sessions` (call/meeting orchestrator + provider adapters), `models`, `usage`, `store`,
`workflows`. Entrypoints `apps/api`, `apps/worker`, `apps/web`. Each package: `domain/ ports/ adapters/ api/`.
`import-linter` enforces layers and package independence; a small stdlib script plus ESLint `max-lines`
enforces <= 100 lines per file in CI (Ruff has no such rule, verified).

Intelligence layer (build order): plan-act-verify loop with persisted checkpoints -> cache-stable context
and compaction -> tool search (always-loaded core + deferred catalogue; pgvector index for non-Anthropic
models) -> Postgres memory (working/episodic/semantic/procedural, permission-aware) -> sub-agents only for
parallel high-value work -> human approval gates by risk tier -> evals as a merge gate (20 golden tasks at
MVP, ~200 later, injection suite included).

Tenancy: a Python backend on the Postgres owner role bypasses RLS, so tenant scope is enforced in code via
a dedicated app role plus RLS as a second layer, both tested.

## 5. Phased roadmap
| Phase | Scope | Exit evidence |
|---|---|---|
| M0 Foundations | Scaffold, CI gates (format/lint/types/tests/100-line/secret scan), ADRs, spikes: DBOS on Supabase pooler, Pydantic AI approvals, Recall output media, LiveKit voice loop | Spikes pass or ADR changes the choice |
| M1 Agent core | Identity/tenancy, tasks, policy + approvals + audit, `ModelProvider` + ledger + budgets, MCP gateway (remote), plan-act-verify, eval suite in CI | One verified end-to-end tool task with audit proof |
| M2 Wedge MVP | Web app, voice session (own app), meeting capture by upload/own session, summary -> actions -> approval -> execute via 3-5 integrations, consent/AI-disclosure UX, billing, waitlist | Design partners use it weekly |
| M3 v1 | Disclosed Recall bots on Zoom/Meet/Teams, speak-on-invite, team workspaces, memory, BYOK + model choice + admin policy, sandboxed Playwright browser tasks, stylised avatar via Simli (metered) | Per-meeting COGS and retention targets met |
| M4 Platform | **Coding capability** (sandbox per task via `Sandbox` port, repo tools via MCP, SWE evals), STDIO MCP + desktop companion, curated directory, SDK/API, EU open-weight tier | Security review passed per feature |
Before M2 heavy build: 15-20 design-partner interviews and a priced waitlist (>= 5% conversion target).

## 6. Unit economics (verified inputs, our estimates)
Core platform: ~$35 / $225 / $905 per month at 50 / 1k / 10k users (~$0.22 per MAU at 1k).
Voice minute: ~$0.04 typical (voice-only); avatar adds $0.01 (Simli) to $0.40 (premium vendor).
Agent tasks on Sonnet 5.5: ~$0.008 Q&A, ~$0.15 10-step browser task, ~$0.12 one-hour meeting summary.
Plans: Free / Pro $20-24 / Team $30-40 per seat; avatar minutes metered at 2-3x COGS, never bundled.
Targets: >= 65% blended gross margin, AI COGS <= 25-30% of revenue; hard per-task and per-user budget caps.

## 7. Security and compliance baseline
Fifteen ranked controls (report 04): policy outside the model, hash-bound human approvals, AI disclosure and
consent flow, tenant isolation, vault for secrets, MCP gateway hardening (allowlist, tool-hash quarantine,
no token passthrough), untrusted-content handling, SSRF-safe egress, disposable browser sandboxes,
audit chain, budgets and kill switch, encryption and tested restore, retention/crypto-shredding, generated
media marking, supply-chain hygiene. EU AI Act Art. 50 applies since 2 Aug 2026: disclosure and marking are
day-one features. No voiceprints, face recognition or emotion inference in MVP. Counsel review is required
before launch (DPIA, recording consent, bot ToS, likeness rights). SOC 2: build aligned controls now, audit later.

## 8. Risks
Name collision (Tavus; tavieri.ai); platform bot blocking (Teams auto-blocks external bots); consent
litigation; big-tech bundling; avatar cost; vendor price moves; Anthropic first-party has no EU pinning (use
Bedrock/Vertex EU) and a $500/mo Start spend cap to raise before launch; DBOS-on-pooler and Fly drain
behaviour are unverified and covered by M0 spikes.

## 9. Team and spend discipline
Roles, names and conventions: `CLAUDE.md`, `docs/CONTRIBUTING.md`. Model policy is binding: Sonnet default,
Haiku for mechanical work, Opus only with justification, Fable never. Reports stay short, scopes narrow.

## 10. Needed from the owner (batched; defaults apply if unanswered)
1. **Name:** do you own tavieri.ai? Rename if counsel confirms Tavus risk? (default: keep "Tavieri" as the working name)
2. **Accounts** (create or connect when M0 starts): Supabase (EU org), Fly.io, Vercel, Sentry, Grafana Cloud, Recall.ai, LiveKit Cloud, Simli, Anthropic (raise spend cap), OpenAI.
3. **Residency and market:** EU/DACH first with EU-only data? (default: yes)
4. **Budgets:** monthly infra ceiling and per-user voice/avatar budget (default: infra <= $250/mo until launch).
5. **Avatar:** stock/synthetic only, or the user's own likeness? (default: stock/synthetic only)
6. **Counsel:** budget and appetite for privacy/AI-Act/trademark review before launch.
7. **Confirm** the "<= 100 lines per file" reading of your code rule.
