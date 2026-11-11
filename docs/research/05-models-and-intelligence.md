# 05 - Models and agent intelligence (model layer, routing, memory, evals, cost)

Author: AI systems architect. Date: 2026-11-11. All sources accessed 2026-11-10/11 unless noted.
Method note: official pages were read through a fetch tool that summarises pages; figures marked UNVERIFIED could not be confirmed. Model names (Fable/Opus/Sonnet/Haiku 5.x, GPT-6.x, Gemini 4) are as printed on the vendor pages today.

## 1. Key findings (sourced)

**Frontier models exist at four price/intelligence tiers per vendor, all 1M-context.**
- Anthropic (platform.claude.com/docs/en/about-claude/pricing and /models/overview): Fable 5.2 $8 in / $40 out per MTok (cache read $0.20); Opus 5.5 $4/$20 (read $0.20); Sonnet 5.5 $2/$10 (read $0.10); Haiku 5.5 $0.10/$0.50 for prompts <=100k tokens, **5x higher above 100k**. 1M context, 128K output, adaptive thinking, text+image input, tool use. Batch -50%. Mythos 5.x is limited-availability (verification programme) - ignore. Newer tokenizer yields ~30% more tokens than Sonnet 4.6 and earlier.
- OpenAI (developers.openai.com/api/docs/pricing, /models): GPT-6 Astra $10/$50 (cached $1); GPT-6.1 Sol $2/$10 (cached $0.10); GPT-6.1 Terra $1/$5; GPT-6 Luna $0.10/$0.50. 1.05M context, 128K output. Realtime: gpt-realtime-2.1 audio $32 in / $64 out per MTok, mini $10/$20; GPT-Live-1 is listed as the premier voice model but **its price is UNVERIFIED**.
- Google (aistudio.google.com/docs/pricing, /docs/models): Gemini 4 Carbon and Argon $2/$10 now, **rising to $4/$20 on 1 Dec 2026**; 3.8 Flash $0.75/$3.75 (-> $1.50/$7.50 on 1 Jan 2027); 3.5 Flash-Lite $0.30/$2.50. 1M context, text/image/audio/video/PDF input. Live API (3.8 Live): audio in $0.005/min, audio out $0.018/min. A secondary source says Carbon is "not publicly available" (geotoolbox.ai) although priced - treat Carbon GA status as UNVERIFIED. Scheduled price rises mean Gemini is not a safe cost anchor.
- Open weights: DeepSeek (api-docs.deepseek.com) `deepseek-flash` V4.1 $0.15 in (cache hit $0.003) / $0.60 out off-peak, double at peak (01-04 and 06-10 UTC weekdays), 1M context, tool calls + vision; `deepseek-v4-pro` $0.66/$1.98, no vision. Kimi K3.1 Pro API $2/$10 (cached $0.20), 1M context; open-weights release status is contradictory across sources (UNVERIFIED), K3 licence is custom with revenue/user thresholds. Qwen3.8-27B is Apache 2.0 (eweek, the-decoder); Qwen3.8-Max (2.4T) is not, with a $50M-revenue licence clause. GLM-5.3, Mistral Large 4 (Mistral Large listed at $0.5/$1.5 on mistral.ai/pricing), Llama 4 Scout (10M context, no index score) also exist. Mistral hosting location is not stated on its pricing page (UNVERIFIED).

**Independent benchmarks (be sceptical: harness and effort setting change rankings).**
- Artificial Analysis Intelligence Index (artificialanalysis.ai, 9 Nov): Fable 5.2 65 ($26.54/task at max effort, 62 at $9.08 high), Carbon 60 ($7.76), Astra 57 ($6.24), GPT-6.1 Sol 55 ($1.27), Kimi K3.1 Pro 46 ($3.68, best open), Qwen3.8 Max 34, DeepSeek V4.1 Flash 33 ($0.60), GLM-5.3 32, Mistral Large 4 26. Via geotoolbox.ai (6 Nov, secondary): Opus 5.5 59 ($10.04), Sonnet 5.5 55 ($10.02). **Same score 55: Sol costs 8x less per task than Sonnet 5.5.**
- Terminal-Bench 4.0, Vals.ai mini-swe-agent (9 Nov): Fable 5.2 = Opus 5.5 65.15%, Sonnet 5.5 64.14%, Kimi K3.1 Pro 63.64% ($4.57/test), Carbon 63.13%, Astra 59.6%, Sol 55.05% ($1.72), DeepSeek V4.1 Flash 31.82% ($0.54), Mistral Large 4 26.3%. Official TB 4.1 differs (Opus 5.5 65.8% with Claude Code, Sol 60.9% with Codex) - ranking depends on harness (arXiv 2605.23950, "Stop comparing LLM agents without disclosing the harness").
- Computer use, OSWorld-Verified (Developers Digest 17 Oct; BenchLM 29 Oct): Claude Fable 5 ~86%, Qwen3.8 Max 86.1%, Opus 5 83.4%, best open MiniMax-M3 75.2%. No current GPT/Gemini rows found. tau-bench: fragmented across versions, no trustworthy single ranking found (UNVERIFIED claim: Fable 5 79.9% tau2-airline). Arena text (6 Nov): Carbon 1526, Argon 1523, Opus 5.5 1505 - preference, not correctness.
- Conclusion: Anthropic leads the long-horizon agentic/computer-use evidence; OpenAI Sol is the cost-efficiency leader at mid-high capability; open weights trail by ~10-20 index points but Kimi is near-frontier on Terminal-Bench. Nobody should be hard-wired; re-run our own evals every model release.

**Data residency (matters for EU customers).**
- Anthropic first-party API: `inference_geo` accepts only `us` or `global`; workspace geo only `us` (platform.claude.com/docs/en/manage-claude/data-residency). **No EU-pinned inference on the first-party API.** EU options: Amazon Bedrock EU inference profiles or single-region endpoints (Frankfurt, Ireland, Paris, etc.; regional endpoints +10%), or Vertex EU multi-region (lists "most Anthropic Claude models"). Bedrock Messages endpoint lacks server-side tools (code exec/web search/fetch), MCP connector, programmatic tool calling, batches, and server-side fallback; supports prompt caching, thinking, memory, computer/browser tools, and tool search (docs list it as stable on Bedrock).
- OpenAI: `eu.api.openai.com`, regional processing incl. Realtime and Responses, +10%, requires ZDR/modified abuse monitoring approval. Google Vertex: EU multi-region incl. Gemini 4 Argon, 3.8 Flash, 3.8 Live (not Carbon); pricing premium UNVERIFIED. OpenRouter: `eu.openrouter.ai` in-region routing; BYOK goes to your own deployment's region.

**Rate limits (Anthropic, platform.claude.com/docs/en/api/rate-limits).** Start tier: Opus/Sonnet/Haiku 5.5 4,000 RPM, 2M ITPM, 400K OTPM; Fable 5.2 4,000 RPM, 500K ITPM, 100K OTPM; **monthly spend cap $500 (Start), $1,000 (Build), $200,000 (Scale)**. Cache reads do not count toward ITPM. OpenAI/Google limits UNVERIFIED.

**Abstraction layer facts.**
- LiteLLM: SDK + self-hosted proxy, "100+ LLMs"; proxy has virtual keys, budgets (key/team/team-member/internal user/end-user "customer" with `budget_duration`), TPM/RPM limits, fallbacks, caching, needs Postgres; per-model budgets are Enterprise; SSO/audit logs Enterprise (docs.litellm.ai). BYOK: not documented on the pages read (UNVERIFIED). **Security: LiteLLM 1.82.7 and 1.82.8 on PyPI were malicious (24 Mar 2026, TeamPCP; credential exfiltration; ~3 hours live; 1.82.6 clean)** (helpnetsecurity.com/2026/03/25, bleepingcomputer, netspi).
- OpenRouter: BYOK free up to $25k/month then 5%; ZDR routing; EU endpoint; automatic provider fallback; credit purchase fee percentage not visible (UNVERIFIED).
- Vercel AI Gateway: zero markup including BYOK; budgets per team/project/key/member but **budgets cover only system-credential spend, BYOK metered separately** and exceeded budget returns 402 even for BYOK; provider/model fallbacks; ZDR controls; works from any infrastructure with an API key (vercel.com/docs/ai-gateway).
- Cloudflare AI Gateway: caching, rate limiting, retries/fallback, analytics; BYOK, spend limits, pricing not covered on page read (UNVERIFIED). Portkey: OSS gateway self-hosted; Production $49/month (100K logs); budgets/semantic cache/SSO Enterprise; OSS licence not stated on page (UNVERIFIED).
- Pydantic AI: `provider:model` strings, `ModelFallback`, native tools exposed per model profile, per-attempt OTel spans, Logfire; MCP client/evals/durable execution/licence not confirmed from page read (UNVERIFIED; check before adopting).

**What makes the agent smart beyond the base model (all Anthropic-engineering/doc sourced unless stated).**
- Multi-agent research system: lead + sub-agents beat single agent by 90.2% on Anthropic's internal eval; agents use ~4x tokens of chat, multi-agent ~15x; token usage alone explained 80% of BrowseComp variance; start evals with ~20 realistic queries; LLM judge (0-1 score + pass/fail) matched humans; resume from checkpoints; rainbow deployments (anthropic.com/engineering/multi-agent-research-system).
- Context engineering: compaction, structured note-taking, sub-agents returning condensed summaries, just-in-time retrieval, minimal non-overlapping tools (anthropic.com/engineering/effective-context-engineering-for-ai-agents).
- Tool search: selection accuracy degrades beyond 30-50 tools; typical 5-server setup ~55k tokens of definitions, tool search cuts >85%; up to 10,000 deferred tools; custom embedding-based search via `tool_reference`; `defer_loading` tools cannot carry `cache_control`; keep 3-5 hot tools non-deferred; **not available in Managed Agents** (docs). Server-side, so other vendors need our own equivalent.
- Memory tool: client-side `/memories` file directory that we map to Postgres; pairs with context editing (`clear_tool_uses`, beta header `context-management-2025-06-27`) and server-side compaction (`compact_20260112`); multi-session pattern: initializer session writes progress log + feature checklist, one feature at a time, mark complete only after end-to-end verification.
- Overhead: computer-use toolset ~4.5k tokens, browser-use toolset ~6.6k tokens per request (pricing page).
- RouteLLM (arXiv 2406.18665): learned strong/weak router claims >2x cost cut at equal quality - a 2024 result on older models; use as precedent, not as drop-in.

**Coding-phase infrastructure (verified prices).** E2B: Hobby $0 with $100 one-off credit, 1h sessions, 10 concurrent; Pro $150/month, 24h sessions, 100 concurrent; $0.000014/vCPU-s + $0.0000045/GiB-s (default 2 vCPU/4 GiB ~ $0.17/h). Daytona: $0.0504/vCPU-h, $0.0162/GiB-h, $200 free credit (2 vCPU/4 GiB ~ $0.17/h). Modal: $0.00003942/core-s (1 core = 2 vCPU) + $0.00000667/GiB-s (~ $0.24/h for that shape), $30/month free. Anthropic code-execution tool: 1,550 free container-hours/month then $0.05/h; free when used with web search/fetch. Managed Agents: $0.08 per running session-hour plus tokens. Compute is ~$0.2/hour: tokens, not sandboxes, dominate cost.

## 2. Numbers

### Model tiers for routing (USD per MTok; input / cached read / output)
| Tier | Primary | Alternate (other vendor) | Cheap/open fallback |
|---|---|---|---|
| Max intelligence | Opus 5.5 4/0.20/20 (Fable 5.2 8/0.20/40 for hardest) | GPT-6 Astra 10/1/50 | Kimi K3.1 Pro 2/0.20/10 |
| Balanced | Sonnet 5.5 2/0.10/10 | GPT-6.1 Sol 2/0.10/10 | DeepSeek V4 Pro 0.66/0.022/1.98 |
| Fast | Haiku 5.5 0.10/0.01/0.50 | GPT-6 Luna 0.10/0.01/0.50 | DeepSeek V4.1 Flash 0.15/0.003/0.60 |
| Realtime voice | gpt-realtime-2.1-mini audio 10/0.30/20 | Gemini 3.8 Live ~$0.005/min in, $0.018/min out | - |

### Per-task cost model (my estimates; assumptions below; cache write 1.25x input)
| Task | Assumed tokens | Fast (Haiku 5.5) | Balanced (Sonnet 5.5) | Max (Opus 5.5) |
|---|---|---|---|---|
| Simple Q&A | 1k new in, 2k cached in, 0.6k out | $0.0004 | $0.008 | $0.016 |
| 10-step browser task | 170k cached read, 30k new written, 6k out (screenshots included) | $0.009 | $0.15 | $0.30 |
| 1-hour meeting summary + actions (transcript ~15k tokens, 2 passes, 5 tool calls) | 35k new in, 25k cached, 5k out | $0.006 | $0.12 | $0.25 |

Caveats: browser tasks with retries/replans commonly cost 2-3x; Haiku is likely too weak for open-web browser agents (Terminal-Bench evidence shows small models collapse: GPT-6 Luna 13.6%); STT/transcription and realtime voice costs are extra. Realtime voice example (verified per-minute rates): 1h meeting, agent speaking 10% of the time on Gemini 3.8 Live ~ $0.30 + $0.11 = ~$0.41/h of audio, excluding text/tool tokens.

Monthly COGS for a light user (20 browser tasks, 10 meetings, 300 Q&A): Balanced $6.7, Max $13.5. Heavy agentic users are 5-10x. A routed mix (Fast for Q&A/summarising chunks, Balanced default, Max on escalation) targets ~$4-6.

## 3. Recommendations

### 3.1 Abstraction: thin owned port, native adapters, optional gateway
**Decision: build and own a small `ModelProvider` port in Python; do not make a third-party proxy the core.** Reasons: (a) the provider-specific features that make the agent smart - Anthropic prompt caching with cache-breakpoint placement, adaptive thinking/effort, `defer_loading` tool search, memory tool, context editing/compaction, computer/browser toolsets, `inference_geo` - are exactly what OpenAI-compatible abstractions flatten; (b) the LiteLLM PyPI compromise shows the risk of putting a fast-moving dependency with all provider keys in the hot path; (c) metering and caps are core product logic (per-user cost caps, plans) that we want in our Postgres ledger.
- Adapters: `anthropic_native` (official SDK; direct, Bedrock Mantle, Vertex variants for EU), `openai_responses` (OpenAI, also Responses-compatible hosts), `openai_compatible` (Gemini compat endpoint, DeepSeek, Kimi, Qwen, Mistral, vLLM/self-hosted), `gateway` (OpenRouter or Vercel AI Gateway for the long tail and BYOK-by-proxy).
- Capability flags per model (not per provider) in a registry: `tool_search`, `prompt_cache`, `thinking`, `vision`, `computer_use`, `max_context`, `eu_resident`, `price`. The agent runtime asks for capabilities; when absent, we substitute our own implementation (e.g. embedding tool search over pgvector, own compaction, own cache-friendly prompt layout).
- Port (names only): `ModelProvider.generate`, `.stream`, `.count_tokens`, `.capabilities`, `.price_card`; types `ModelRequest`, `ModelResponse`, `ToolSpec`, `ToolCall`, `Usage` (input/cached_read/cache_write/output/audio), `ProviderOptions` (typed escape hatch, e.g. `AnthropicOptions.effort`), `ModelRef` (provider+model+region+credential_ref), `CredentialResolver` (platform vs BYOK), `UsageLedger.record`, `BudgetGuard.check`, `ModelRegistry`, `Router.select`, `FallbackPolicy`.
- Agent loop: evaluate Pydantic AI for the loop/fallback/OTel (re-verify MCP, evals, durable-execution claims first); otherwise own a small loop. Durable execution of long tasks belongs to the workflow engine chosen by the platform architect, not the model layer.
- Gateways: **use OpenRouter (EU endpoint) or Vercel AI Gateway only as one adapter** for catalogue breadth/BYOK and as a break-glass fallback. If a self-hosted LiteLLM proxy is ever wanted for virtual-key budgets, run the pinned, cosign-verified Docker image as an isolated service with only that service's keys, never as a library inside the agent process. Cloudflare/Portkey add little over our own ledger at MVP.
- BYOK: user keys stored envelope-encrypted, resolved per request by `CredentialResolver`, never logged, never sent to other providers; BYOK users are billed a platform fee/seat, not tokens; we still meter tokens for display. Warn when a BYOK model lacks capabilities (flags). Note gateway budgets do not cap BYOK spend (Vercel), so enforce caps in our own `BudgetGuard`.
- Fallback: client-side retry with jitter, then same-tier cross-vendor fallback only if the workspace policy allows (data residency, no-training). Bedrock lacks server-side fallback, so client-side is the portable design anyway.

### 3.2 Routing and user choice
- Tiers exposed to users: **Fast / Balanced (default) / Max / Auto**, plus "pin a model" for Pro and BYOK. Auto = per-step rules, not an ML router at launch.
- Per-role defaults: router/classifier/title/summary-chunk = Fast; executor (tool loop) = Balanced; planner, verifier on high-risk actions, hard-failure escalation = Max (different vendor for the verifier where policy allows, to decorrelate errors - design judgement, validate with evals); realtime voice = realtime model (voice model only talks and delegates reasoning to the agent runtime via tool call); meeting summary = Balanced with Fast chunk pre-pass.
- Cascade: start Balanced; escalate to Max on verifier fail, repeated tool errors, or low self-reported confidence; cap escalations per task.
- Business plans: admin policy object = allowed providers/models, max tier, region pin (EU-only => Bedrock EU/Vertex EU/OpenAI EU only), BYOK-only or platform-only, per-seat and per-workspace budgets, "no training/ZDR required", disable unverified-capability models.
- **Eval-gated routing:** a model/tier may serve a task class only if it passes the regression suite: non-inferiority within a set margin (e.g. <=2 points pass-rate, with confidence interval over >=100 tasks per class), plus cost/task and p95 latency budgets. Promote via shadow runs on consented/redacted traffic, then canary 5-10%, auto-rollback on online proxies (task success, retries, thumbs, escalation rate). Judge = deterministic checks first, LLM judge second, calibrated on a human-labelled subset. Always compare cost per *successful task*, not per token: verbosity and retries change totals (Sol vs Sonnet cost gap above).

### 3.3 Intelligence layer patterns (adopt in this order)
1. Plan -> act -> verify loop: explicit plan object with success criteria, step checkpoints persisted in Postgres, verifier checks criteria against tool evidence before "done"; resume from checkpoint after failure.
2. Context engineering: cache-stable prompt prefix (system, tools, policies first), cache breakpoints, tool-result clearing, compaction, notes file per task; results of sub-agents returned as short summaries.
3. Tool scale: always-loaded core (3-5 tools) + deferred catalogue. On Anthropic use `tool_search_*` with `defer_loading` (MCP: `default_config` on `mcp_toolset`); for other models use our own `search_tools` over a pgvector index of tool name/description/args (the same index feeds the Anthropic custom `tool_reference` path). Namespace tool names by server; treat MCP metadata as untrusted.
4. Memory: working (context + task notes), episodic (per-task/per-meeting summaries with timestamps), semantic (user/org facts and preferences in Postgres + pgvector + FTS, hybrid retrieval, per-tenant row-level isolation, write-gating with confirmation for sensitive facts, expiry), procedural (saved playbooks). Expose to Claude as the `memory` tool backed by Postgres for parity, and as retrieval tools for other models.
5. Sub-agents only for parallelisable, high-value work (they cost ~15x chat tokens); synchronous at first.
6. Human-in-the-loop: approval gates keyed by action risk tier from the policy engine (outside the model); plans previewable; irreversible actions (send, pay, delete) always confirm by default.
7. Evals as a merge gate: start with ~20 golden tasks per capability (Q&A grounding, 10-step browser, meeting summary+actions, MCP tool selection at 200+ tools, long-horizon), grow to ~200; metrics: success, steps, tokens, $/success, tool-error rate, safety (prompt-injection suite). CI runs a cheap subset on every PR and the full suite nightly and on any model/prompt/router change; regression beyond margin blocks merge. Tooling (Pydantic evals, Inspect, Langfuse/Braintrust) UNVERIFIED - pick after a one-day spike.

### 3.4 Sequencing
- **MVP (first release):** port + `anthropic_native` + `openai_compatible` adapters; tiers Fast/Balanced/Max; usage ledger + per-user budget guard; plan-act-verify loop; prompt caching; tool search with deferred MCP tools; Postgres memory files; 20-task eval suite in CI; Anthropic account moved past the $500 Start spend cap before launch.
- **Phase 2:** sub-agents, compaction/context editing, episodic+semantic memory (pgvector), BYOK, admin policies, cross-vendor fallback, eval-gated Auto routing, EU path (Bedrock EU or Vertex EU) with capability gaps handled, realtime voice model abstraction.
- **Phase 3 (coding):** ephemeral sandbox per task (E2B or Daytona behind a `Sandbox` port; ~$0.17/h; start with Hobby credit/free tier), repo tools via GitHub MCP, test runner, network-egress allowlist, no secrets inside sandbox; long-horizon harness using progress log + feature checklist and end-to-end verification before marking done; run it on the same runtime and policy layer. Add SWE-style evals before enabling. Consider self-hosted Firecracker only when sandbox spend justifies it.
- **Phase 4:** EU-hosted open-weight tier (Qwen3.8-27B Apache 2.0, or Kimi if weights/licence confirmed) for cost-sensitive and sovereign customers; learned router only if eval data shows it beats rules.

### 3.5 Margins
- Meter in credits mapped to tokens; show cost estimates before expensive tasks; hard per-task token/step caps and per-user monthly caps (BudgetGuard before every call); default Balanced not Max.
- Cache aggressively: stable prefixes (reads are 5-10% of input price; Sonnet/Opus 5.5 read at 5%); Haiku 5.5 requires staying under 100k tokens/prompt.
- Cheapest viable tier per step; Batch (-50%) for non-interactive work (digests, post-meeting reports).
- Target AI COGS <=25-30% of revenue on typical usage; price Max/agent-heavy usage via credits or BYOK. Avoid single-vendor price shocks (Gemini 4 doubling 1 Dec; DeepSeek peak surcharge) by keeping >=2 vendors per tier.

## 4. Risks
- Benchmark volatility and vendor/harness bias; model names change monthly (retirement commitments: Opus 5.5 not before 22 Sep 2027, Sonnet 5.5 28 Sep 2027, Haiku 5.5 7 Oct 2027, Fable 5.2 9 Nov 2027).
- Abstraction leakage: non-Anthropic models lack tool search/caching parity; BYOK users may pick weak models and blame the product.
- EU residency: Anthropic first-party cannot pin EU; EU path loses server-side tools and uses cloud-partner terms.
- Supply-chain risk in gateway/agent libraries (LiteLLM incident): pin hashes, lockfiles, isolate keys.
- Prompt injection through tool output and MCP metadata; policy must stay outside the model.
- Cost blow-ups from loops and multi-agent fan-out; Anthropic Start tier spend cap $500/month throttles launch (429 without retry-after).
- Open-weight licence ambiguity (Kimi K3.1 weights, Qwen Max clause, Llama licence UNVERIFIED).

## 5. Where the initial docs are wrong/outdated/optimistic
- They list "OpenAI APIs among supported options" (MASTER doc line 205) and otherwise do not define a model layer: no tiering, routing, BYOK, metering, eval gating or capability matrix. Needs a dedicated ADR.
- Nothing in the docs budgets per-task token cost; agentic tasks cost 4x (single agent) to 15x (multi-agent) chat tokens.
- "Provider adapters" implies equivalence; in practice caching, thinking, tool search and computer use are vendor-specific and drive quality and cost.
- Assumes realtime voice and the reasoning agent are one model; the voice model should be a thin front-end delegating to the agent runtime.

## 6. Open questions for the owner
1. Target price points and plans (to fix the AI COGS budget); is a BYOK-only or BYOK-discount plan wanted at launch?
2. EU-only data residency requirement for launch customers? (determines Bedrock EU / Vertex EU contracts and AWS/GCP accounts).
3. Accounts: Anthropic API org (request tier increase above $500/month cap), OpenAI, Google Cloud/Vertex, AWS Bedrock; budget for eval runs (~$200-500/month initially, estimate).
4. Appetite for Chinese-vendor models (Kimi, DeepSeek, Qwen APIs) given customer procurement rules; self-hosting only if yes.
5. Legal sign-off on storing users' provider keys (BYOK) and on sending meeting audio/transcripts to third-party model vendors.
