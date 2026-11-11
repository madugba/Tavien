# 06 - Zero-cost track (M0-M2)

Author: Ravi Kapoor (devops-sre). Sources fetched 2026-11-11; Claude dev tokens ignored per owner.
Pages that failed or were silent are marked UNVERIFIED. Architecture decisions D1-D11 are unchanged
(Postgres, FastAPI modular monolith, Next.js, DBOS, model-agnostic, BYOK); only the hosting plan changes.

## 1. Plan

**Local-first (always $0).** `docker compose up` (Postgres+pgvector, API, worker, web) plus
`supabase start` (local Auth/Storage). CI and evals run against this stack, never against hosted services.
Remote environments exist only for demos and design partners.

| Layer | Zero-cost choice | Notes |
|---|---|---|
| Data + Auth | **Supabase Free**, EU region (eu-central-1), 1 project | 500 MB, no backups, pauses after 1 week idle |
| API + durable worker | **One always-on Fly.io Machine** (shared-cpu-1x, 512 MB, fra) running API and DBOS worker in one process | List price about $3.7/mo (256 MB is $2.19; 512 MB interpolated, UNVERIFIED), under Fly's $5 waiver, so billed $0. Needs a card |
| Web | **Next.js as static export / client-rendered on Cloudflare Workers static assets** (free) | Avoids the 10 ms CPU cap on SSR. Vercel Hobby is not allowed (below) |
| CI | GitHub Actions free credit | Public-repo minutes are free; private repos use the $12 credit |
| Errors / traces | Sentry Developer + Grafana Cloud free via OTLP | Limits from report 02 |
| Backups | Nightly `pg_dump` in a GitHub Actions cron, age-encrypted artifact, weekly restore test into local Postgres | Supabase Free has no backups |
| Models | BYOK + capped trial credit; free-tier models for demos on synthetic data only (section 4) | |

**Single cheapest "durable enough" worker: the one Fly Machine above.** It is the same Dockerfile and
`fly.toml` we will scale later (no migration), restarts on crash, and DBOS resumes workflows from Postgres
after any restart. Its constant DB polling also keeps the Supabase project from ever idling into a pause.
Fallback if Fly's waiver does not apply to our org (UNVERIFIED): **Oracle Always Free** Ampere A1
(2 OCPU, 12 GB, 200 GB disk, 10 TB egress), truly $0 but with capacity errors ("out of host capacity"),
idle reclamation (7-day p95 CPU, network and memory all under 20%) and self-managed OS. Use it only as plan B.

**What breaks at the free limits**
- Supabase: 500 MB DB fills (vectors and audit chain grow fastest); 1 week idle means pause; no backups, so
  a bad migration is unrecoverable except from our own dump; 5 GB egress; 2 active projects total.
- Fly 512 MB: OOM under concurrent agent runs or large Playwright/STT work; heavy jobs cannot run there.
  Deploys restart the single Machine, so sessions drop (acceptable for demos; drain logic comes later).
- Cloudflare Workers Free: 100k requests/day (Error 1027 beyond), 10 ms CPU per request, 50 subrequests.
- Sentry: 5k errors/mo, 1 user. Grafana: 14 d retention, 10k series. GitHub: private minutes run out.
- No staging environment: one Supabase project; staging is local.

## 2. Free-tier facts (verified 2026-11-11 unless marked)

| Service | Free terms | Commercial use / verdict | URL |
|---|---|---|---|
| Supabase Free | 500 MB DB, pause after 1 week inactivity, 2 active projects, 5 GB egress (+5 GB cached), 1 GB storage, 50k MAU, no backups | Pricing page silent. EU regions exist (report 02); that free projects can pick them is UNVERIFIED | https://supabase.com/pricing |
| Vercel Hobby | 100 GB transfer, 1M invocations, 4 h Active CPU | **Non-commercial personal use only.** Commercial = any deployment for the financial gain of anyone involved, including a paid employee writing the code, or advertising a product or service. Not usable for Tavieri | https://vercel.com/docs/limits/fair-use-guidelines |
| Cloudflare Workers Free | 100k req/day, 10 ms CPU/req, 50 subrequests, 64 MiB | Limits page silent on commercial use; ToS check UNVERIFIED | https://developers.cloudflare.com/workers/platform/limits/ |
| Fly.io | No free tier for new orgs; $1 trial for 7 days (no card); card required afterwards; monthly bill under $5 waived for a personal org; shared-cpu-1x 256 MB $2.19/mo | Waiver for non-personal orgs UNVERIFIED | https://docs.fly.io/about/pricing |
| Render Free | Spins down after 15 min (about 1 min wake), 750 h/mo, no disk; free Postgres 1 GB expires after 30 days | "Do not use them for production applications." Rejected | https://render.com/docs/free |
| Cloud Run | 2M requests, 180k vCPU-s, 360k GiB-s, 1 GB NA egress per month (about 50 vCPU-hours, so not an always-on worker); eligible regions UNVERIFIED | Scale-to-zero API only; cannot host the DBOS recovery loop for free | https://docs.cloud.google.com/free/docs/free-cloud-features |
| Koyeb | No free compute listed; free Postgres "Free 5h" (0.25 vCPU, 1 GB, Frankfurt), semantics unclear | Rejected | https://www.koyeb.com/pricing |
| Oracle Always Free | A1 2 OCPU/12 GB, 2 micro VMs (1 GB), 200 GB block, 10 TB egress; idle reclamation | Plan B only | https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm |
| Neon Free | 1 GB/project, 100 CU-h/mo, suspend after 5 min idle, 6 h history, 5 GB transfer; EU regions UNVERIFIED | A constantly polling DBOS worker would burn 100 CU-h in about 2 weeks at 0.25 CU. Rejected as primary DB | https://neon.com/pricing |
| GitHub Actions | Public repos free; private $12/mo credit at $0.006/min (about 2,000 min) | Report 02, fetched 2026-11-10 | https://docs.github.com/en/billing/managing-billing-for-your-products/about-billing-for-github-actions |
| Sentry / Grafana | 5k errors, 5M spans, 1 user / 10k series, 50 GB logs, 14 d, 3 users | Report 02, fetched 2026-11-10 | https://sentry.io/pricing/ , https://grafana.com/pricing/ |
| Gemini API free tier | Flash and Flash-Lite models (3.x) free of charge. **Free-tier content is used to improve Google products**; paid tier is not. EU/EEA restriction not confirmed (page partly read) | Demos with synthetic data only | https://aistudio.google.com/docs/pricing |
| OpenRouter `:free` | 20 req/min; 50 req/day, or 1,000/day after buying 10 credits one time; BYOK requests not gated | Fine for smoke tests, too flaky for evals | https://openrouter.ai/docs/api-reference/limits |
| Groq Whisper | Page shows Developer-plan base limits: whisper-large-v3(-turbo) 20 RPM, 2k RPD, 7.2k audio-s/h, 28.8k audio-s/day. Free-plan numbers sit in the account limits page (UNVERIFIED) | Good for demo STT | https://console.groq.com/docs/rate-limits |
| Deepgram credit | Not fetched. UNVERIFIED | Skip until needed | - |
| faster-whisper (local) | Open source, runs on a dev machine, $0 (licence and speeds UNVERIFIED) | Default dev/eval STT | - |

## 3. Triggers to a paid plan

| Component | Trigger (observable signal) | Move to | Monthly cost after |
|---|---|---|---|
| Supabase | First design partner uploads real data, OR DB over 400 MB (80%), OR we need a restorable backup | Pro (7-day backups, no pause) | $25 |
| Fly worker | OOM or p95 memory over 80% on 512 MB, OR more than one Machine, OR bill over $5 | 1 GB Machine, then API and worker split | $6.70, about $13 split (about $10 in report 02) |
| Web host | Over 70k req/day on Workers Free, OR SSR needed (CPU over 10 ms), OR commercial-use clause bites (Vercel) | Workers Paid $5 (or Vercel Pro $20/seat if we choose Vercel) | $5 to $20 |
| Sentry | Over 4k errors/mo or a second seat | Team | $29 |
| Grafana | Over 8k series or need over 14 d retention | Pro | $19 + usage |
| GitHub Actions | Private minutes over about 1,600/mo | Pay per minute ($0.006) or make the repo public | about $5 |
| PITR | A paying customer demands RPO under 24 h | Supabase PITR add-on | $100 |
| Staging | First paying customer | Second Supabase project (Pro) | $25 |

Cumulative estimate: **$0** (M0 to first partner), **about $35** (first design partner with real data:
Supabase Pro + Fly, matches report 02), **about $70** (first paying customer: add Workers Paid, Sentry),
**about $225** at about 1k MAU (report 02). Staging, PITR and Vercel seats are extra.

## 4. Model and voice cost strategy

- **BYOK first.** Design partners paste their own provider key; our ledger enforces per-session and
  per-user caps regardless (hard stop, not an alert).
- **Trial credit:** a prepaid, separately capped provider key (suggest $5 per partner, $25/mo global hard
  cap, kill switch in the ledger). This is the only model spend we fund; needs owner budget approval.
- **Evals:** PR CI replays recorded fixtures (zero tokens). Nightly golden-task runs use dev-covered models.
  Free tiers (Gemini Flash, OpenRouter `:free`) serve smoke tests and demos with synthetic data only,
  never partner data, because Gemini's free tier trains on content.
- **Voice, cheapest prototype later:** push-to-talk in our own web app: browser records audio, API
  transcribes with Groq Whisper (demo) or local faster-whisper (dev), LLM answers, browser SpeechSynthesis or
  local Piper (UNVERIFIED) speaks. No WebRTC, no LiveKit. Barge-in and streaming come later.
- **Defer until a design partner or revenue justifies:** LiveKit (when full-duplex streaming is required),
  Recall.ai (about $0.50-0.60/h, only when a partner needs bots in Zoom/Meet/Teams; M2 uses upload or
  own-session capture per the spec), Simli (avatars are M3). Meter them at 2-3x COGS when added.

## 5. Risks

1. Fly waiver unconfirmed for company orgs (worst case about $3.7/mo); Oracle plan B has capacity risk.
2. Supabase Free: no backups and a 500 MB cap; mitigated by nightly dump plus restore test, not eliminated.
3. DBOS on Supabase (Supavisor session mode or direct IPv6) is still an M0 spike; Neon is not a fallback.
4. Cloudflare free caps and OpenNext gaps; static export limits Next.js features (no SSR, no middleware).
5. Gemini free tier trains on content; one slip leaks partner data. Enforce with a model-policy flag.
6. Single Machine means deploys drop sessions and there is no HA; acceptable only before paying users.
7. Free-tier terms change on 30 days' notice (Google) or silently; re-verify at each phase gate.
8. Owner decisions needed: approve a card on Fly (and Oracle if plan B), a $25/mo trial-credit cap, and
   confirm Vercel is dropped.
