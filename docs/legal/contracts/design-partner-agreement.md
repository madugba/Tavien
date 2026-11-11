> **DRAFT - NOT FOR SIGNATURE.** Prepared by Hannah Weiss (AI security architect) on 2026-11-11.
> Not legal advice. Requires counsel review (consumer/B2B law, liability limits, local mandatory terms).

# Design Partner Agreement (Pilot)

**Effective date:** [DATE]

**Between** [LEGAL ENTITY NAME], [ADDRESS] ("**Tavieri**") **and** [PARTNER LEGAL NAME], [PARTNER ADDRESS]
("**Partner**"). This agreement covers a free, limited pilot of pre-release software. It is for businesses
only; individuals acting as consumers may not sign.

## 1. Purpose and scope
1.1 Tavieri gives Partner access to pre-release features of the Tavieri AI teammate (the "**Pilot
Service**"): capture of Partner-run calls or uploads, summaries, proposed actions, human approval and
execution through connected tools. Features, limits and the number of Partner users are listed in
**Schedule 1** and may change as the product evolves.
1.2 Partner will use the Pilot Service for its own internal business purposes, for at least [one] named
contact meeting [weekly/fortnightly], and will not resell, benchmark publicly, or reverse engineer it.
1.3 The Terms of Service (`../terms-of-service.md`), Acceptable Use Policy (`../acceptable-use-policy.md`)
and Privacy Policy (`../privacy-policy.md`) apply to the extent not inconsistent with this agreement;
this agreement prevails on conflict.

## 2. Free use and no commitment
2.1 The Pilot Service is free during the Pilot Term. No fees are owed. Tavieri may introduce pricing for
later versions and will offer Partner [a discount / early-access terms - PRODUCT OWNER TO DECIDE]; neither
party must buy or sell.
2.2 Tavieri may change, suspend or withdraw any feature at any time and has no obligation to release a
commercial product or to maintain compatibility, uptime or support beyond reasonable efforts.

## 3. Feedback
3.1 Partner will give candid feedback (usage, bugs, ideas, interviews) ("**Feedback**").
3.2 Partner grants Tavieri a worldwide, perpetual, irrevocable, royalty-free, sublicensable, non-exclusive
licence to use Feedback for any purpose, without attribution. Feedback must not include Partner
Confidential Information or personal data; if it does, Partner is responsible and Tavieri will delete it
on request. Tavieri gives no right in Tavieri technology by receiving Feedback. Moral rights are waived or
not asserted to the extent permitted by law.
3.3 With Partner's prior written approval (email suffices) Tavieri may name Partner and quote Feedback
publicly. Absent approval, no publicity.

## 3A. Ownership
Tavieri keeps all rights in the Pilot Service. Partner keeps all rights in its content, recordings,
transcripts, prompts and connected-tool data ("**Partner Content**") and grants Tavieri a limited licence
to process it solely to provide the Pilot Service (and to keep security and audit logs). Tavieri does not
use Partner Content to train models. [COUNSEL: confirm model-provider terms support this before engaging
any provider; see `subprocessor-list.md`.] Outputs are provided as-is; Partner is responsible for
reviewing them, and ownership of AI-generated output is uncertain in some jurisdictions (see
`contributor-and-ip-assignment.md` clause 9 for the analysis), but as between the parties Tavieri assigns
to Partner whatever rights it holds in output generated from Partner Content.

## 4. Confidentiality
Each party's non-public information and the existence and content of the Pilot Service roadmap, and
Partner Content, are confidential. The parties' confidentiality obligations are those of the **Mutual NDA**
(`mutual-nda.md`), which is incorporated and, if not separately signed, applies as if set out here for the
term plus [3] years. Pre-release features, bugs and security findings in the Pilot Service are Tavieri
Confidential Information.

## 5. Data handling
5.1 **Scope limits.** During the pilot Partner will not upload or capture special-category data, children's
data, payment card data, health data, or data of employees without a documented works-council or
consultation process where required. Use is limited to [PARTNER-OWN SESSIONS / UPLOADS AS PER SCHEDULE 1]
(no external-meeting bots until Schedule 1 says so).
5.2 **Roles.** Tavieri processes personal data in Partner Content as Partner's processor under the Data
Processing Agreement (`data-processing-agreement.md`), which Partner must sign before any personal data
is uploaded. Until the DPA is signed, Partner will use synthetic or non-personal data only.
5.3 **Sub-processors.** Only vendors with status "engaged" in `subprocessor-list.md` will receive Partner
Content; Partner accepts the engaged vendors listed in Schedule 2 at signature.
5.4 **Retention and deletion.** Default retention is [30] days; Partner may delete at any time; on
termination Tavieri exports on request and then deletes Partner Content within [30] days (DPA clause 11).
5.5 **Security events.** Each party notifies the other of incidents affecting Partner Content as in DPA
clause 9. Security vulnerabilities found by Partner are reported to [SECURITY EMAIL] and kept confidential.
5.6 **BYOK.** If Partner supplies model API keys, `byok-addendum.md` applies.

## 6. Recording consent and AI disclosure (Partner responsibility)
6.1 Partner is solely responsible for lawfully recording, transcribing and analysing any call, including:
informing all participants that the call is recorded and that an AI system is participating or analysing;
obtaining consent where required (for example all-party consent states such as California and
Pennsylvania, Quebec and other jurisdictions, and EU and UK rules on lawful basis, transparency and
employee information); and honouring objections by excluding or deleting that participant's data.
6.2 Partner will not disable or obscure the in-product disclosure and consent features, will not use the
Pilot Service to impersonate a person, to mislead anyone that they are speaking to a human, or to create
voice or likeness of any real person without that person's documented consent, and will comply with the
Acceptable Use Policy and `../ai-transparency-and-meeting-consent.md`.
6.3 Partner will give Tavieri on request the text of notices and consents it uses, for support and
compliance review, and indemnifies Tavieri for third-party claims arising from Partner's failure to meet
this clause, [subject to the cap in clause 8 - COUNSEL].

## 7. No warranty
THE PILOT SERVICE IS PRE-RELEASE, PROVIDED "AS IS" AND "AS AVAILABLE", WITH NO WARRANTY OF ANY KIND
(EXPRESS, IMPLIED OR STATUTORY), INCLUDING MERCHANTABILITY, FITNESS, ACCURACY OF AI OUTPUT,
NON-INFRINGEMENT AND AVAILABILITY, TO THE EXTENT PERMITTED BY LAW. AI output may be wrong, incomplete or
offensive; Partner reviews every proposed action before approval. Partner must keep its own copies of any
data it needs. [COUNSEL: German and EU law restrict disclaiming liability for intent, gross negligence,
injury, and cardinal duties; adjust governing-law terms and avoid unenforceable disclaimers.]

## 8. Liability
8.1 To the extent permitted by law, neither party is liable for indirect, consequential or lost-profit
damages, and Tavieri's total liability under this agreement is limited to [EUR/USD 100] (the Pilot Service
being free). Nothing limits liability for fraud, wilful misconduct, gross negligence, death or personal
injury, breach of confidentiality, or liability that cannot be limited by law.
8.2 Partner's liability for indemnities, and breach of clause 6, is limited to [AMOUNT / UNLIMITED -
COUNSEL].

## 9. Term and termination
9.1 The pilot starts on the Effective date and lasts [90] days (the "**Pilot Term**"), extendable by
written agreement.
9.2 Either party may terminate at any time on [7] days' written notice (email suffices), or immediately
for material breach, security risk or legal requirement, including misuse of the Pilot Service.
9.3 On end of the Pilot Term or termination: access ends, Partner Content is exported on request and
deleted per clause 5.4, and Feedback licence, confidentiality, clauses 3, 6, 7, 8, 10 and 11 survive.

## 10. General
Entire agreement for the pilot (with the documents referenced); changes in writing; no assignment without
consent except to a successor of the business; notices by email to the addresses below; electronic
signature is valid and counterparts allowed; if a term is unenforceable the remainder stands. Governing
law and venue: [GOVERNING LAW / VENUE]; mandatory consumer or local law is unaffected.

## 11. Compliance
Each party complies with applicable sanctions, export and anti-corruption law and will not use the Pilot
Service in sanctioned jurisdictions. Partner will not use the Pilot Service for law-enforcement, credit
or employment decisions, biometric identification or emotion inference.

## Signatures
| For [LEGAL ENTITY NAME] (Tavieri) | For [PARTNER LEGAL NAME] |
|---|---|
| Name/title: [SIGNATORY NAME/TITLE] | Name/title: ______________________ |
| Signature: __________________ | Signature: __________________ |
| Date: [DATE] | Date: ______________________ |
| Notice email: [EMAIL] | Notice email: _______________________ |

## Schedule 1 - Pilot scope
Pilot Term: [DATE] to [DATE]. Partner users (max [5]): [NAMES/EMAILS]. Features: [e.g. upload or own-session
capture, summary, proposed actions, approval, execution through [3-5] named integrations]. Excluded: external
meeting bots, voice and avatar, [OTHER]. Support: email, best effort, [DAYS]. Feedback cadence: [WEEKLY CALL].
Data categories permitted: [LIST]. Retention: [30] days.

## Schedule 2 - Engaged sub-processors at signature
[SNAPSHOT OF ROWS WITH STATUS "ENGAGED" FROM `subprocessor-list.md`; EMPTY IF NONE YET]
