> **DRAFT - NOT LEGAL ADVICE - NOT FOR PUBLICATION.** Prepared 2026-11-11 by Hannah Weiss (security-architect, AI team member) for review and finalisation by qualified counsel. Bracketed items `[LIKE THIS]` are open decisions; see `README.md` checklist.

# Tavieri Privacy Policy

**Effective date:** [EFFECTIVE DATE] | **Last updated:** [DATE]

## 1. Who we are
Tavieri is provided by **[LEGAL ENTITY NAME]**, [ADDRESS] ("Tavieri", "we", "us").
Privacy contact: **[DPO or PRIVACY CONTACT EMAIL]**. EU representative (Art. 27 GDPR): [EU REPRESENTATIVE, IF REQUIRED]. UK representative: [UK REPRESENTATIVE, IF REQUIRED]. Quebec privacy officer: [NAME/TITLE, CONTACT].

Tavieri is an AI agent platform. You connect tools (for example through the Model Context Protocol, "MCP"), optionally bring your own model API keys, and Tavieri can listen, summarise, propose actions and, after your approval, carry them out. It can optionally speak and appear as an AI avatar in meetings.

## 2. Our roles
| Situation | Our role |
|---|---|
| Account, billing, security, usage analytics, support, marketing | **Controller** |
| Content you or your organisation put into Tavieri (files, prompts, meeting audio, transcripts, tool data) | **Processor / service provider** for you or your organisation, which decides why the content is processed. [COUNSEL: confirm; a Data Processing Addendum (DPA) applies to business customers] |
| Content about meeting participants who are not Tavieri users | The Tavieri customer who invited the AI is responsible for notice and legal basis; we process on its behalf. We are controller only for our own consent/disclosure records. [COUNSEL: confirm split] |

If you are an individual using Tavieri in a team workspace, your administrator may see and control your workspace data. Ask your administrator about their privacy practices.

## 3. Personal data we process
- **Account data:** name, email, password hash or sign-in identity, organisation, role, language, settings.
- **Billing data:** plan, invoices, tax details; card data is handled by [PAYMENT PROCESSOR] and not stored by us.
- **Content you provide:** prompts, uploaded files, task instructions, outputs, memory items you or the agent save.
- **Meeting data:** audio, live and stored transcripts, chat messages seen by the AI, participant names and join/leave times, summaries, action items. Video is not recorded by default. [CONFIRM defaults]
- **Voice and avatar data:** your microphone audio during voice sessions; avatar selection. We use **stock or synthetic avatars only**. We do not create voiceprints, face templates or any biometric identifier, and we do not infer emotions.
- **Connected-tool data:** data returned by tools you connect (calendar, email, drive, CRM and similar) to the extent the agent is permitted to access it, and OAuth tokens that you grant.
- **Your model API keys (BYOK):** see section 7.
- **Usage and technical data:** IP address, device and browser type, log events, per-task cost and token counts, audit records of actions the agent took and who approved them, error reports.
- **Communications:** support requests and feedback.
- **Cookie data:** see the Cookie Policy.

We do not intentionally collect special-category data (health, beliefs, biometrics and so on). Meeting content may contain it incidentally. Do not use Tavieri to process it unless your organisation has a lawful basis for doing so.

## 4. Why we use data, and our legal bases
| Purpose | GDPR / UK GDPR basis |
|---|---|
| Provide the service you request: run agents, transcribe, summarise, execute approved actions | Contract (Art. 6(1)(b)) |
| Security, abuse prevention, fraud, audit trails, service integrity | Legitimate interests (Art. 6(1)(f)); legal obligation where applicable |
| Billing, tax, accounting | Contract; legal obligation |
| Product analytics and improvement (opt-in only) | Consent (Art. 6(1)(a)) |
| Marketing emails | Consent, or legitimate interests where law allows, always with opt-out |
| Recording/transcribing meetings where we act for a customer | Customer's basis (typically legitimate interests or consent) |
| Enforcing our terms, defending claims | Legitimate interests |
| Complying with law and requests from authorities | Legal obligation |

Where we rely on consent you may withdraw it at any time without affecting earlier processing. Where we rely on legitimate interests you may object (section 10).

**We do not use your content to train AI models** (ours or third-party providers'), and we contract with model providers on terms that prohibit training on your content. [CONFIRM per provider contract and per BYOK route; see 7]

## 5. AI and meeting specifics
- **Disclosure first.** The AI joins a meeting only when invited by a user and, where the platform requires it, approved by the host. It identifies itself as an AI by name, by chat message and, if it speaks, by voice, as described in our *AI Transparency and Meeting Consent Notice*. Capture starts only after that notice.
- **Participants can object.** Any participant can ask for capture to stop; the host can remove the AI at any time and we stop capture on removal. Users must obtain any further consent that local law requires (see Terms).
- **Media retention is minimal.** Raw audio is off by default and, when enabled, deleted after [X] days or sooner on request. Transcripts and summaries are kept per section 8.
- **Human approval.** The agent proposes actions; actions with external effect run only after approval, per policy you or your administrator set. We do not make solely automated decisions with legal or similarly significant effects about individuals.
- **AI outputs can be wrong** and may reflect personal data from your content. Review before relying or sharing.
- **Marking.** Synthetic audio and avatar video are labelled as AI-generated and carry machine-readable marking where technically supported (EU AI Act Art. 50).
- **Untrusted content.** Web pages, files and tool output are treated as data, not instructions, and policy is enforced outside the model.

## 6. Who we share data with
- **Subprocessors** that host and run the service (cloud, database, meeting-bot, voice, avatar, model providers, email, payments, error monitoring). Current list, locations and purposes: **[SUBPROCESSOR LIST URL]**. We give notice of changes as described there. [Planned categories are listed in `README.md`.]
- **Model providers.** Your content is sent to the model provider that runs the model you or the router select. With BYOK, to the provider you chose, under your account.
- **Tools you connect.** Data flows to and from third-party tools you authorise; their policies apply.
- **Your organisation** (administrators) and meeting participants you share outputs with.
- **Professional advisers, auditors, and authorities** where legally required.
- **Corporate transactions** (merger, acquisition), with notice.

We **do not sell** personal data and do not "share" it for cross-context behavioural advertising. [CONFIRM]

## 7. Bring-your-own-key (BYOK) handling
Keys you provide are encrypted at rest in a secrets vault, decrypted only in memory at the moment of a model call, never shown again after entry (only a masked tail), never written to logs, prompts, URLs or process arguments, and never sent to anyone but the provider they belong to. You can revoke or rotate a key at any time; deletion removes it from the vault immediately. Usage under your key is billed by your provider to you; we show estimates only. We recommend scoped keys with spend limits. Your provider's terms and privacy practices govern data they receive.

## 8. Retention
| Data | Default retention |
|---|---|
| Account data | Life of account, then deleted within [30] days (backups up to [35] days) |
| Content, transcripts, summaries, memory | Until you delete it or [90] days after account closure |
| Raw meeting audio / video (if enabled) | [X] days, then deleted |
| Audit records of agent actions | [X] years; personal payloads are separable and erased on request, the integrity chain keeps only hashes |
| Billing records | [7] years (legal) |
| Security and application logs | [90] days |
| Consent and disclosure records | [X] years (evidence) |
| Marketing preferences | Until withdrawn |

[ENGINEERING TO CONFIRM every figure before publication.] We may keep data longer where required for law or to defend claims.

## 9. International transfers
Our primary hosting is in the **EU** ([HOSTING REGION, e.g. Frankfurt]). Some subprocessors and model providers may process data outside the EEA/UK, including in the **United States**. We rely on, as applicable: adequacy decisions (including the EU-US Data Privacy Framework and UK Extension for certified recipients), Standard Contractual Clauses and the UK Addendum/IDTA, and supplementary measures. Copies of safeguards are available on request. [COUNSEL: the DPF adequacy decision is under appeal at the EU Court of Justice (Case C-703/25 P); keep SCCs as a fallback. Confirm EU-only processing options for customers.]

## 10. Your rights
Email **[DPO or PRIVACY CONTACT EMAIL]** or use in-product controls. We respond within one month (GDPR/UK) or 45 days (US states, extendable), and may need to verify your identity. Business customers' end users should usually contact their organisation first.

**EEA and UK.** Access, rectification, erasure, restriction, portability, objection (including to legitimate-interest processing and direct marketing), withdrawal of consent, and the right not to be subject to solely automated decisions with significant effects. Complain to your supervisory authority ([EU LEAD AUTHORITY, IF ANY]) or, in the UK, the Information Commissioner's Office (or its successor body). [COUNSEL: confirm UK regulator name and complaints-handling steps under the Data (Use and Access) Act 2025; UK law lets us ask you to complain to us first.]

**US states** (including California, Colorado, Connecticut, Virginia, Texas, Oregon and others with comprehensive laws). Know/access, delete, correct, portability, opt out of sale, targeted advertising and profiling in furtherance of significant decisions, limit use of sensitive personal information (we use it only to provide the service), non-discrimination, and appeal a refusal (we respond within the period set by your state). We honour browser opt-out signals such as Global Privacy Control. You may use an authorised agent. California categories collected in the last 12 months correspond to section 3; sources and recipients to sections 3 and 6; we do not sell or share. [COUNSEL: confirm thresholds, CCPA notice-at-collection formatting, employee/B2B data scope, and the CCPA automated decision-making and risk-assessment rules (ADMT obligations from 1 Jan 2027).]

**Canada.** Access, correction, withdrawal of consent, challenge compliance, and complaint to the Office of the Privacy Commissioner of Canada. **Quebec** (Law 25): additionally, information about automated processing, data portability in a structured commonly used format, and de-indexation or cessation of dissemination where applicable; our privacy officer is listed in section 1. We conduct privacy impact assessments before transfers out of Quebec. We send commercial electronic messages only with consent that complies with **CASL**, with identification and a working unsubscribe. [COUNSEL: federal Bill C-36 is pending; Quebec French-language requirement.]

## 11. Security
Encryption in transit and at rest, tenant isolation, vaulted secrets, least-privilege per-tool grants that are revocable and audited, human approval gates, access logging and tested backups. No system is perfectly secure. We notify authorities and affected people of breaches as the law requires. See [SECURITY PAGE URL]. We do not claim any certification (for example SOC 2 or ISO 27001) we do not hold.

## 12. Children
Tavieri is for people aged **16 and over**. We do not knowingly collect personal data from anyone under 16 and delete it if we learn we have. Parents may contact us at the address above. [COUNSEL: confirm 16 floor against Member State ages of digital consent and COPPA/US state youth provisions.]

## 13. Cookies
We use strictly necessary cookies and, only with your consent, analytics cookies. See the Cookie Policy.

## 14. Automated decisions and profiling
Tavieri's agents act on your instructions. We do not profile you to make decisions with legal or similarly significant effects. Routing between AI models is based on task type and cost, not on personal characteristics.

## 15. Changes
We will post changes here, update the date, and give notice by email or in-product for material changes at least [30] days before they apply, except where the law requires otherwise.

## 16. Contact
**[LEGAL ENTITY NAME]**, [ADDRESS] | **[DPO or PRIVACY CONTACT EMAIL]**
