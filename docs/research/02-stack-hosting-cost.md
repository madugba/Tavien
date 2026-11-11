# 02 - Stack, hosting and core-platform cost

Author: Principal Engineer / Platform & Cost Architect. Sources fetched 2026-11-10 unless noted.
Scope: core platform only (excludes LLM, speech, avatar, WebRTC/SFU media, email, payment fees).
EUR to USD assumed 1.10 (assumption, not verified). Anything not fetched is marked UNVERIFIED.

## 1. Key findings (sourced)

### 1.1 Supabase suitability
- **Pricing/tiers** (https://supabase.com/pricing): Free $0 (500 MB DB, 1 day logs, no backups, paused after 1 week idle). Pro from $25/mo (7-day daily backups, 8 GB DB, 250 GB egress, 100 GB storage, 100k MAU, $10 compute credit covering one Micro). Team from $599/mo (14-day backups). Compute add-ons: Micro $10 (1 GB, shared), Small $15 (2 GB), Medium $60 (4 GB), Large $110 (2 dedicated vCPU, 8 GB), XL $210, 2XL $410, 4XL $960 (https://supabase.com/docs/guides/platform/compute-and-disk). PITR add-on $100/mo per 7 days; WAL shipped every 2 min so worst-case RPO about 2 min; enabling PITR stops daily backups (https://supabase.com/docs/guides/platform/backups).
- **EU residency**: eu-west-1/2/3, eu-central-1 (Frankfurt), eu-central-2 (Zurich), eu-north-1 (https://supabase.com/docs/guides/platform/regions). Choose a specific region, not the "general" one.
- **Auth from FastAPI**: verify JWTs locally against the JWKS endpoint (`/auth/v1/.well-known/jwks.json`, ES256/RS256, edge-cached about 10 min). Legacy HS256 shared-secret verification is "strongly" discouraged (https://supabase.com/docs/guides/auth/jwts). Auth MAU is effectively free at our scale (100k included, then $0.00325/MAU).
- **RLS with a Python backend**: a direct connection as `postgres` bypasses RLS (`bypassrls`), so the backend must enforce tenancy itself; RLS then protects only direct client paths (Realtime, PostgREST) (https://supabase.com/docs/guides/database/postgres/row-level-security). Wrap `auth.uid()` in `(select ...)` and index policy columns.
- **Pooling**: persistent backends use direct (5432, needs IPv6 or $4/mo IPv4 add-on) or Supavisor session mode (IPv4, free). Transaction mode (6543) breaks prepared statements, LISTEN/NOTIFY, session advisory locks (https://supabase.com/docs/guides/database/connecting-to-postgres). Micro: 60 direct / 200 pooler clients.
- **Realtime**: Pro 500 concurrent / 500 msg/s; Pro without spend cap and Team 10,000 / 2,500; messages $2.50 per 1M beyond 5M (limits page + pricing). **Cost cliff** if agent token streams go through it.
- **Edge Functions**: Deno, 256 MB, 2 s CPU/request, 150 s idle timeout, 400 s wall clock on paid; docs say push heavy long jobs to workers (https://supabase.com/docs/guides/functions/limits).
- **Queues**: pgmq-based, visibility window, archive; docs list no limits or DLQ (https://supabase.com/docs/guides/queues). **Vault**: encrypted secrets in Postgres; a manual pg_dump/restore into a new project loses the key unless copied (https://supabase.com/docs/guides/database/vault).
- **Exit path**: Postgres is plain; `supabase db dump` and restore-to-new-project documented. Self-hosting exists (Docker Compose) but excludes managed backups/PITR, branching, Management API and is single-project (https://supabase.com/docs/guides/self-hosting). License statement UNVERIFIED on that page. Auth users, RLS policies and Storage metadata move as SQL; Realtime/Edge are the only non-portable parts.
- **Spend cap** (Pro) does not cover compute, PITR, IPv4 (https://supabase.com/docs/guides/platform/spend-cap).

**Use Supabase for**: Postgres + pgvector (HNSW/IVFFlat; dimension limits UNVERIFIED here), Auth, small Storage, coarse-grained Realtime (task status), backups. **Do NOT use it for**: real-time media or token streaming (message-priced, payload-based), long-running/agent/browser workers (Edge limits above), business logic via PostgREST/RPC (kills testability and the 100-line modularity), bulk recording storage (egress $0.09/GB; prefer R2, price UNVERIFIED this session), the primary workflow engine UI.

### 1.2 Backend hosting (API + bursty, hour-long worker sessions)
| Option | Verified facts | Fit |
|---|---|---|
| **Fly.io Machines** (https://docs.fly.io/about/pricing) | shared-cpu-1x 256 MB $2.19/mo, 1 GB $6.70, performance-1x 2 GB $33; extra RAM $6/GB/mo; per-second billing; stopped Machine pays only rootfs $0.15/GB; Europe egress $0.02/GB; dedicated IPv4 $2; autostop/autostart built in, stop cold start about 2 s, suspend few hundred ms (https://docs.fly.io/launch/autostop-autostart/); metrics autoscaler is a DIY app. Prices are US-region (EU region delta UNVERIFIED). | Best: WebSockets, scale to near-zero, one Machine per browser/meeting session via API. Ops: low-medium. Managed Postgres exists ($38+) but we keep Supabase. |
| **Cloud Run** | Request timeout max 60 min; WebSockets are long HTTP requests, cut at the timeout, clients must reconnect; instance with open socket bills as active; scales to zero; concurrency up to 1,000 (https://docs.cloud.google.com/run/docs/triggering/websockets). Price about $0.000018/vCPU-s, $0.000002/GiB-s instance-based per search snippets only: UNVERIFIED (official page would not load). | Fine for stateless API; poor for sessions >60 min or background workers (needs min instances). |
| **Hetzner + Coolify/Docker** (https://www.hetzner.com/cloud/regular-performance; https://coolify.io/docs/get-started/introduction) | CPX22 2 vCPU/4 GB EUR 19.99 DE (16.99 FI); CPX32 4/8 GB EUR 35.99; CPX42 8/16 GB EUR 69.99; 20 TB traffic included; monthly cap. Coolify: Docker deploys from git, multi-server, no documented autoscaling; you own OS patches, backups, firewall. | Cheapest per vCPU, no scale-to-zero, highest ops burden. |
Also checked: Railway ($20/vCPU-mo, $10/GB-mo, egress $0.05/GB, Pro $20, one EU region Amsterdam - https://railway.com/pricing, /docs.railway.com/reference/regions); Fargate US-East about $0.0405/vCPU-h, $0.0044/GB-h, EU UNVERIFIED (https://aws.amazon.com/fargate/pricing/); Render prices UNVERIFIED (page not retrievable); Cloudflare Containers $0.000020/vCPU-s active, $0.0000025/GiB-s, needs Workers Paid $5 (https://developers.cloudflare.com/containers/pricing/), GA/EU placement UNVERIFIED.

### 1.3 Web hosting
- Vercel: Hobby is "personal, non-commercial"; Pro $20/seat with $20 credit, 1 TB transfer then from $0.15/GB, 10M edge requests then $2/M, Active CPU from $0.128/h (https://vercel.com/pricing). Cliffs: per-seat, bandwidth, SSR/streaming Active CPU.
- Cloudflare Workers Paid $5/mo, 10M req, no egress charge; Next.js via OpenNext (Next 16 supported; Node middleware unsupported; 10 MiB gz Worker limit) (https://developers.cloudflare.com/workers/platform/pricing/, https://opennext.js.org/cloudflare).
- Self-host on Hetzner/Fly: cheapest, more ops.

### 1.4 Durable background work
| Option | Facts | Verdict |
|---|---|---|
| **DBOS Transact (Python)** | MIT, v3.5.0 (2026-11-10), durable workflows/steps checkpointed in Postgres, retries, durable sleep, queues, timeouts, no extra server; Conductor optional (https://pypi.org/project/dbos/, https://docs.dbos.dev/python/tutorials/workflow-tutorial) | **Pick.** $0 extra infra; fits pause/resume/approval (days-long waits). Young: PoC against Supavisor session mode. |
| Procrastinate | v3.10.0, Postgres job queue: retries, periodic, locks, priorities, async (https://procrastinate.readthedocs.io/en/stable/) | Runner-up for plain jobs. No workflow state. |
| pgmq/Supabase Queues | visibility window, archive; no DLQ/limits documented | Too low-level. |
| Hatchet | MIT; Postgres required, RabbitMQ optional; cloud $30/M runs, 1M free (https://hatchet.run/pricing) | Next upgrade if we need a UI/engine service. |
| Temporal Cloud | $50/M actions pay-go ($25 volume), $150 trial credit; Business from $500/mo (https://temporal.io/pricing) | Upgrade path at enterprise/multi-team scale; self-host is heavy. |
| Inngest | Free 50k executions; Pro $99 incl. 1M, each step = execution (https://www.inngest.com/pricing) | Per-step billing penalises agent loops. |
| Upstash Redis | 500K commands free, $0.20/100k (https://upstash.com/pricing/redis) | Not durable-by-default; unnecessary. |
Keep a `WorkflowEngine` port so DBOS can be swapped for Temporal.

### 1.5 Observability / CI / secrets
- Sentry: Developer free (5k errors, 5M spans, 1 user); Team $29/mo (https://sentry.io/pricing/). Grafana Cloud free: 10k series, 50 GB logs/traces, 14 d, 3 users; Pro $19 + usage (https://grafana.com/pricing/). Plan: OpenTelemetry SDK in FastAPI/workers (OTLP) to Grafana Cloud; Sentry for errors (Sentry-OTel integration UNVERIFIED this session).
- GitHub Actions: private repos $12/mo (Free) or $18 (Team) credit, Linux 2-core $0.006/min; public repos free (https://docs.github.com/en/billing/managing-billing-for-your-products/about-billing-for-github-actions).
- Secrets: Fly/Vercel/Supabase native secret stores + GitHub Actions secrets with OIDC; per-user MCP tokens in Supabase Vault (envelope key outside DB) or app-level envelope encryption. Infisical/Doppler UNVERIFIED (not fetched).

### 1.6 Quality tooling (versions from PyPI, 2026-11-10)
uv 0.13.7, ruff 0.17.4, mypy 2.4.1, pyright 1.1.414, ty 0.0.100 (pre-1.0, advisory only), pytest 9.2.0, pytest-asyncio 1.4.1, import-linter 2.16, tach 0.35.3, FastAPI 0.162.4.
Pick: **uv workspace + ruff (lint+format) + pyright strict (or mypy strict, one of them) + pytest + import-linter**; ESLint + TypeScript strict + Vitest + Playwright on web.

**100-lines rule - tested:**
- Ruff 0.17.4 has 974 rules and **no max-lines/too-many-lines rule** (`ruff check --select PLC0302` returns "Unknown rule selector"). Only per-function rules (PLR0912/0913/0915).
- Pylint 4.1.3 works: `pylint --disable=all --enable=C0302 --max-module-lines=100` flagged a 120-line file ("120/100"). Slow, adds a dependency.
- ESLint `max-lines` (default 300; options max, skipBlankLines, skipComments) for TS: `"max-lines": ["error", {"max": 100, "skipBlankLines": true, "skipComments": true}]` (https://eslint.org/docs/latest/rules/max-lines). Biome equivalent UNVERIFIED.
- **Recommended single enforcer**: a 26-line stdlib script `tools/check_max_lines.py` (tested: flags 120-line file, rc=1) run via pre-commit and CI over `git ls-files` for `.py/.ts/.tsx`, counting physical lines. It is language-agnostic and uses one rule for both stacks (no skipping blanks/comments, so the limit cannot be gamed). Add ESLint max-lines too for editor feedback. Exclude only generated files (`*_pb2.py`, OpenAPI client) via an explicit allowlist file reviewed by the principal engineer.
- Architecture boundaries: import-linter `layers` + `forbidden` contracts (tested; needs `include_external_packages = true` for third-party bans; a violating import was reported with file:line).

## 2. Numbers (core platform, USD/month; estimates)
Assumptions: prototype = 50 users, <=3 concurrent sessions; launch = 1k MAU, ~15 peak sessions, 3k sessions/mo x 45 min, 5k browser tasks x 5 min; growth = 10k MAU, ~150 peak sessions, 30k sessions x 45 min, 50k browser tasks, 50 GB DB, 500 GB egress. Sessions are I/O-bound, ~10 per vCPU. Fly session Machine shared-cpu 2 GB approx $0.0176/h (derived: $6.70 + $6 extra GB). Realtime limited to coarse events.

| Stack | Prototype | Launch (1k) | Growth (10k) |
|---|---|---|---|
| **A: Supabase Pro + Fly + Vercel + DBOS + Sentry/Grafana** | about 35 (Supabase 25, Fly 10, Vercel Hobby 0) | about 225 (Supabase Pro+Medium 75, Fly 75, Vercel 2 seats 40, Sentry 29, CI 5) | about 905 (Supabase Large+PITR+overages 265, Fly 410, Vercel 80, obs. 130, CI 20) |
| **B: Hetzner + Coolify, self-run Postgres/auth/backups** | about 27 (1x CPX22 + backup) | about 165 | about 720 (about 4 CPX42 workers, 2 CPX32 API, 2 DB nodes, backup, obs.) |
| Per MAU (A) | n/a | $0.22 | $0.09 |
Core platform cost per meeting-minute at growth is about $0.0002 (Fly compute for sessions); LLM/speech/avatar dwarf it. B saves about $60/mo at launch and about $185/mo at growth, less than a few engineer-hours of ops; it also forgoes managed Auth/PITR/Realtime. A hidden swing: Realtime messages ($2.50/M) and PITR ($100) are the likeliest bill surprises in A.

## 3. Risks
1. DBOS maturity/pooler compatibility (use session-mode/direct connection; PoC).
2. Fly Machines in-session deploys/drains kill hour-long sessions: needs graceful drain + resumable state. Fly EU-region prices and WebSocket behaviour UNVERIFIED.
3. Supabase Realtime/Storage egress cliffs; IPv6/IPv4 connectivity; shared-CPU compute below Large.
4. Vercel seat/bandwidth cliffs; OpenNext gaps if we move to Cloudflare.
5. Vendor concentration (Supabase for Auth+DB): mitigated by plain-Postgres schema and own `Identity` port.
6. 100-line rule can fragment code; counter with package-level cohesion and architecture contracts.

## 4. Recommendation
**Stack A**: Supabase (Postgres + pgvector + Auth + Storage, EU region eu-central-1) as managed data layer only; **FastAPI modular monolith** on **Fly.io** (fra) with one API process group, one workflow-worker group, and session-per-Machine for browser/meeting workers; **Next.js on Vercel Pro** (Hobby only before first external user); **DBOS on the Supabase Postgres** for durable workflows; OTel + Sentry + Grafana free tiers; GitHub Actions with OIDC; uv/ruff/pyright/pytest/import-linter plus the max-lines script.
Rationale: lowest cost and ops for a small team, scale-to-near-zero, no extra queue/Redis infra, exit paths at every layer.
**Runner-up: Hetzner + Coolify** (self-run Postgres/auth). Switch when Fly+Supabase exceeds about $3-5k/mo (savings then 40-60%, UNVERIFIED estimate), or when sustained high-concurrency workers make always-on boxes cheaper, or strict data-sovereignty demands it. Cloud Run only for the stateless API if we ever consolidate on GCP.

### Proposed monorepo (uv workspace + pnpm)
```
tavien/
  apps/api/        # FastAPI entrypoint, DI wiring only
  apps/worker/     # DBOS worker entrypoint
  apps/web/        # Next.js (TypeScript)
  packages/py/
    kernel/        # ids, errors, clock, config, telemetry
    identity/  tasks/  tools/  mcp/  policy/  memory/
    sessions/      # meeting/call orchestrator + provider adapters
    models/        # model routing (vendor-agnostic)
    usage/  store/ workflows/   # metering, DB adapters+migrations, WorkflowEngine port
  packages/ts/     # api-client (generated), ui, config
  tools/           # check_max_lines.py, import-linter config
```
Each Python package: `domain/ ports/ adapters/ api/` with one class/function family per file; import-linter layers (api > adapters > ports > domain; domain imports no framework) and independence between packages except via `ports`.

## 5. Where the initial docs are wrong/outdated/optimistic
- "Redis where required": nothing here needs Redis; Postgres covers queue, state, cache.
- "Temporal only if justified": agree, but the doc leaves the cheaper middle (DBOS/Procrastinate) unnamed; Temporal Cloud pay-go is $50/M actions.
- "Docker and cloud infrastructure": unspecified; Cloud Run's 60-minute WebSocket cap would silently break hour-long sessions.
- Doc lists RLS/tenancy as DB work, but a Python backend on the `postgres` role bypasses RLS, so tenancy must live in code plus a dedicated app role.
- No file-size enforcement mechanism exists in Ruff; the 100-line rule needs the script above.
- Cost is absent: Realtime message pricing and PITR are non-obvious line items.

## 6. Open questions for the owner
1. Approve accounts: Supabase (EU org), Fly.io, Vercel, Sentry, Grafana Cloud, GitHub org with Actions.
2. Monthly infra budget ceiling and whether paying $100/mo PITR is accepted at launch.
3. Hard EU-only residency requirement for user data and recordings (affects Fly region and Vercel/Sentry/Grafana data regions)?
4. Is Vercel Hobby acceptable pre-revenue, or start on Pro ($20/seat)?
