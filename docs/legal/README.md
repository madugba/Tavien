> **DRAFT - NOT LEGAL ADVICE - NOT FOR PUBLICATION.** These documents were prepared 2026-11-11 by Hannah Weiss (security-architect, an AI team member) from public secondary sources and the team's research. **They are drafts, not legal advice.** They must be reviewed, corrected and finalised by qualified counsel in each relevant jurisdiction before publication or use. Release is blocked until the checklist below is cleared.

# Tavieri Legal Documents (drafts)

| File | Purpose |
|---|---|
| `privacy-policy.md` | Controller/processor roles, data, bases, AI/meeting specifics, BYOK, transfers, rights by region |
| `terms-of-service.md` | Contract, BYOK, MCP tools, AI outputs, liability, optional arbitration clause |
| `acceptable-use-policy.md` | Prohibited uses, including covert recording, deepfakes, biometrics |
| `cookie-policy.md` | Strictly necessary cookies plus opt-in analytics |
| `ai-transparency-and-meeting-consent.md` | User notice, participant notice, bot chat and voice consent wording, product rules |

Related not yet drafted: Data Processing Addendum, subprocessor list page, security page, vulnerability disclosure policy, DPIA, ROPA, record of consent evidence. Threat models live in `docs/security/` (to be created).

## Law checked on 2026-11-11 (web search, secondary sources; verify against primary text)
| Topic | Finding | Confidence |
|---|---|---|
| EU AI Act Art. 50 | Applies from 2 Aug 2026. Digital Omnibus on AI moved only the Art. 50(2) marking duty to 2 Dec 2026, for systems already on the market; a new product gets no grace. Code of Practice on AI-content marking final 10 Jun 2026; Commission Art. 50 guidelines reported 20 Jul 2026 | Medium: one July source said the Omnibus was not yet binding; confirm Official Journal publication |
| EU-US DPF | In force. General Court dismissed Latombe (T-553/23) on 3 Sep 2025; appeal C-703/25 P is pending at the CJEU; regulators urge contingency plans | Medium. Keep SCCs as fallback |
| UK | Data (Use and Access) Act 2025 mostly commenced 5 Feb 2026; complaints provisions 19 Jun 2026; PECR fines raised to UK GDPR levels; one source says the ICO became the "Information Commission" on 30 Sep 2026 | Medium/low on the regulator rename |
| US states | About 20 comprehensive state laws; Indiana, Kentucky and Rhode Island effective 1 Jan 2026. California ADMT/risk-assessment/cyber-audit regulations effective 1 Jan 2026; ADMT duties from 1 Jan 2027; risk-assessment submissions from 2028; dates differ across sources | Medium |
| BIPA | 2024 amendment limits damages to one recovery per person; 7th Cir. (Clay v. Union Pacific, 1 Apr 2026) held it retroactive. Exposure is reduced but not removed; we avoid voiceprints entirely | Medium |
| Recording consent | All-party list from secondary sources (CA, FL, IL, MD, MA, MT, NH, PA, WA, DE, NV) not verified against statutes | Low: counsel must verify |
| COPPA | Amended Rule: full compliance since 22 Apr 2026. Mitigation: minimum age 16, no knowing collection under 16 | Medium |
| Canada | PIPEDA still governs federally. Bill C-36 (first reading 15 Jun 2026) would replace it; pending. Quebec Law 25 fully in force, express consent, no size threshold, penalties up to CAD 25M or 4%. CASL private right of action suspended since 2017 (no 2026 confirmation found), regulator enforcement continues | Medium |

## COUNSEL REVIEW CHECKLIST (every open decision and placeholder)
**Entity and identity**
1. [LEGAL ENTITY NAME], [ADDRESS], [EFFECTIVE DATE], [DPO or PRIVACY CONTACT EMAIL], [LEGAL NOTICES EMAIL], [ABUSE CONTACT EMAIL], [SECURITY CONTACT EMAIL].
2. Is a DPO required (Art. 37)? EU Art. 27 and UK representatives? Quebec privacy officer named and published?
3. Name clearance: trademark conflict risk with Tavus (and tavieri.ai ownership); `[TRADEMARKS]` in Terms.

**Roles and bases**
4. Controller/processor split, particularly for non-user meeting participants; need for a DPA, SCC modules and Art. 28 terms with business customers.
5. Legal bases per purpose; legitimate-interest assessments; whether consent is needed for meeting transcription in each market; DPIA before launch (voice, AI, meetings, large scale); AI Act role analysis (provider vs deployer).
6. Works councils and employee-monitoring rules (DACH launch: Germany/Austria telecom-secrecy and criminal recording provisions).

**Meeting consent and recording**
7. Verified all-party-consent statutes; strict explicit opt-in vs notice-and-no-objection (`ai-transparency...` D1); wording of C1-C7; Quebec/Canadian express consent; per-jurisdiction defaults.
8. Platform bot terms (Zoom RTMS, Teams, Meet) and Recall.ai contract terms.
9. Confirm no feature constitutes biometric processing (speaker diarisation must not use voiceprints); BIPA/CUBI/WA/GDPR Art. 9 posture.

**AI-specific**
10. Art. 50 compliance: disclosure, machine-readable marking, avatar being "obviously" synthetic, whether the deepfake duty is triggered by stylised avatars; Omnibus publication status; Code of Practice signature.
11. "We do not train on your content": confirm each model provider's contract and zero-retention terms, including BYOK routes; EU-region options (Anthropic via Bedrock/Vertex EU).
12. Automated decision-making statements (GDPR Art. 22, UK DUAA reforms, CCPA ADMT rules from 2027, Quebec Law 25 s.12.1).
13. Likeness/voice rights if own-likeness avatars are added later (not in these drafts).

**Transfers and processors**
14. EU-US DPF reliance given CJEU appeal C-703/25 P; SCCs/TIAs per subprocessor; UK Addendum/IDTA; hosting region claims (`[HOSTING REGION]`).
15. Publish `[SUBPROCESSOR LIST URL]`; planned categories per spec: Supabase (EU Postgres/Auth/Storage), Fly.io (Frankfurt), Vercel (web), Recall.ai (meeting bots), LiveKit (voice), Simli (avatar), Anthropic and OpenAI (models), Sentry, Grafana Cloud, [PAYMENT PROCESSOR], [EMAIL PROVIDER]. Notice period and objection right for changes.

**Regional rights and notices**
16. California notice-at-collection format, sale/share statement, GPC handling, B2B/employee data scope, applicable thresholds across states; "we do not sell or share" confirmation.
17. UK regulator naming and complaints procedure; EU lead authority.
18. Quebec French-language requirement for policies and consent; CASL consent for marketing emails; PIPEDA breach reporting; Bill C-36 monitoring.
19. COPPA/state youth laws with the age-16 floor; age gate mechanism; EU digital-consent ages; Member-State rules below 16.

**Terms**
20. Payments, taxes, trials, auto-renewal, refunds, EU 14-day withdrawal; consumer vs business segmentation.
21. Liability cap amount and carve-outs; indemnity scope; warranties.
22. Governing law and venue per customer type; **optional arbitration/class-waiver clause: include or omit** (consumer enforceability varies; mass-arbitration risk).
23. Acceptable-use enforcement and appeal process; DSA/notice-and-action obligations if hosting user-visible content.

**Retention and operations (with engineering)**
24. Every `[X]` retention figure; audit-chain erasure approach (crypto-shredding); backup lag; consent-evidence retention.
25. Cookie inventory from a real scan; whether PECR/ePrivacy exemptions can apply to analytics; consent banner design (equal prominence, withdraw anytime).
26. Security claims in the Privacy Policy match controls actually shipped; no SOC 2/ISO claims until audited.
27. Insurance (cyber, tech E&O) and breach notification playbook (GDPR 72h; US states; PIPEDA; Quebec).
