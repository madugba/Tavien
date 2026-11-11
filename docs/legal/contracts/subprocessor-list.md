> **DRAFT - NOT FOR SIGNATURE.** Prepared by Hannah Weiss (AI security architect) on 2026-11-11.
> Not legal advice. Referenced as the agreed sub-processor list in `data-processing-agreement.md`
> (clause 6, Annex III). Source: `docs/superpowers/specs/2026-11-11-tavieri-platform-design.md`.

# Sub-processor List

**Controller-facing entity:** [LEGAL ENTITY NAME], [ADDRESS] | **List version:** 0.1 | **Last updated:** 2026-11-11

## Rules for this list
1. **No vendor receives Customer Personal Data until its status is "engaged" and the 30-day customer
   notice (DPA clause 6.2) has run**, unless the customer is a design partner who has accepted the
   addition in writing. Today every row is "planned / not yet engaged"; no customer data may be sent.
2. **Update this file before engaging any vendor.** Sequence: (a) security review and vendor DPA/SCCs signed;
   (b) fill "Verified" and "Transfer mechanism"; (c) update status; (d) send customer notice; (e) engage.
3. Entries marked [VERIFY] are working assumptions from the design spec and vendor marketing, to be
   confirmed from the signed vendor DPA and the vendor's current documentation before engagement.
4. Bring-your-own-key model providers are not Tavieri sub-processors; see `byok-addendum.md` clause 6.

## Planned sub-processors
| # | Vendor | Purpose | Customer data processed | Processing location | Transfer mechanism | Status |
|---|---|---|---|---|---|---|
| 1 | Supabase, Inc. | Postgres, pgvector, Auth, Storage (system of record) | All content at rest (transcripts, summaries, tasks, audit), account and auth data, embeddings | EU region [CONFIRM Frankfurt] | Intra-EEA hosting; vendor is US-parented, so [SCCs / DPF - VERIFY] for support access | planned / not yet engaged |
| 2 | Fly.io, Inc. | Application and worker compute, session machines | Content in memory and temporary disk during sessions | Frankfurt (fra) [CONFIRM] | [SCCs / DPF - VERIFY] (US-parented) | planned / not yet engaged |
| 3 | Vercel Inc. | Web app hosting and edge delivery | IP addresses, request metadata, session tokens; no stored content by design | Global edge; function region [EU - CONFIRM] | [SCCs / DPF - VERIFY] | planned / not yet engaged |
| 4 | Anthropic, PBC (or Anthropic models via AWS Bedrock / Google Vertex EU, which would then be listed separately) | LLM inference (default model family) | Prompts and tool context containing transcript excerpts and task data | First-party API has no EU pinning per spec; EU via Bedrock/Vertex [DECIDE] | [SCCs; DPF - VERIFY]; zero-retention and no-training terms [VERIFY] | planned / not yet engaged |
| 5 | OpenAI (legal entity [VERIFY]) | LLM inference (OpenAI-compatible adapter, optional model) | As row 4 | [EU data residency for API - VERIFY availability; else US] | [SCCs; DPF - VERIFY]; zero-retention and no-training terms [VERIFY] | planned / not yet engaged |
| 6 | Recall.ai, Inc. | Meeting bot, capture and output media on Zoom, Meet and Teams | Meeting audio, video, participant names, transcript | [EU region - CONFIRM availability and pinning] | [SCCs / DPF - VERIFY] | planned / not yet engaged (M3 scope) |
| 7 | LiveKit, Inc. | Real-time voice and video sessions | Live audio and video streams, room metadata | Cloud region pinning [VERIFY]; global mesh if unpinned | [SCCs / DPF - VERIFY] | planned / not yet engaged |
| 8 | Simli (legal entity and location [VERIFY]) | Avatar video rendering | Synthesised speech audio and avatar frames; no customer likeness (stock or synthetic only per spec) | [VERIFY] | [VERIFY: UK adequacy or SCCs] | planned / not yet engaged (M3 scope, metered feature) |
| 9 | Functional Software, Inc. (Sentry) | Error and performance monitoring | Stack traces, request metadata; content scrubbed before send (verify scrubbing) | EU region [CONFIRM de.sentry.io] | [SCCs / DPF - VERIFY] | planned / not yet engaged |
| 10 | Grafana Labs (Grafana Cloud) | Metrics, logs and traces | Operational telemetry; no content or secrets (logging policy) | EU region [CONFIRM] | [SCCs / DPF - VERIFY] | planned / not yet engaged |

## Candidates not yet selected (add rows when chosen)
Billing and payments provider (M2) [TBD]; transactional email [TBD]; speech-to-text and text-to-speech
vendors if used outside LiveKit plugins [TBD]; third-party tools the customer connects (Google, Microsoft,
CRM) are the customer's own integrations, not sub-processors, and run under the customer's authorisation.

## Change log
| Date | Change | Notice sent |
|---|---|---|
| 2026-11-11 | Initial draft listing ten planned vendors | n/a (none engaged) |

Approved for publication: [SIGNATORY NAME/TITLE] | [DATE]
