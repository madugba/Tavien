> **DRAFT - NOT FOR SIGNATURE.** Prepared by Hannah Weiss (AI security architect) on 2026-11-11.
> Not legal advice. Requires review by qualified counsel in each governing jurisdiction. Bracketed
> items are unknown facts or open decisions. See `README.md` for the signing guide and counsel questions.

# Data Processing Agreement (DPA)

**Version:** 0.1 (draft) | **Effective date:** [DATE]

Between **[LEGAL ENTITY NAME]**, [ADDRESS] ("**Tavieri**", the "**Processor**"), and the customer
identified in the signature block ("**Customer**", the "**Controller**", or where Customer processes on
behalf of its own clients, a "**Processor**" and Tavieri its "**Sub-processor**"). Each a "**Party**".

This DPA forms part of the agreement under which Customer uses the Tavieri service: the Terms of Service
(`../terms-of-service.md`) or a signed order or Design Partner Agreement (`design-partner-agreement.md`)
(the "**Principal Agreement**"). Controller-side notices to individuals are in the Privacy Policy
(`../privacy-policy.md`); this DPA does not replace them. Tavieri acts as an independent controller only
for the account, billing, security and product-analytics data described in that Privacy Policy.

## 1. Definitions
"**Applicable Data Protection Law**" means, to the extent applicable: Regulation (EU) 2016/679 ("GDPR");
the UK GDPR and Data Protection Act 2018; the Swiss FADP; the California Consumer Privacy Act as amended
("CCPA") and other US state privacy laws; the Canadian Personal Information Protection and Electronic
Documents Act ("PIPEDA"), Quebec's Act respecting the protection of personal information in the private
sector ("Law 25") and substantially similar provincial law. "**Customer Personal Data**" means personal
data in Customer Content processed by Tavieri on Customer's behalf, including meeting audio, video,
transcripts, summaries, proposed actions, connected-tool data and prompts. Terms such as "controller",
"processor", "personal data", "personal data breach", "supervisory authority" and "process" have their
GDPR meaning; CCPA terms ("business", "service provider", "business purpose", "sell", "share") have their
CCPA meaning.

## 2. Roles, scope and instructions
2.1 Customer is controller (or processor for its clients); Tavieri processes Customer Personal Data only
on Customer's documented instructions: this DPA, the Principal Agreement, and Customer's configuration and
use of the service (including approvals given in the product). Tavieri will inform Customer without
undue delay if, in its opinion, an instruction infringes Applicable Data Protection Law, and may suspend
the affected processing until resolved.
2.2 Details of processing (subject matter, duration, nature, purpose, data types, data subjects) are in
**Schedule 1**. Customer determines what content is captured and is responsible for the accuracy and
lawfulness of its instructions.
2.3 Tavieri does not use Customer Personal Data to train or fine-tune any model, and will contract with
model sub-processors on terms that prohibit training on, and limit retention of, Customer Personal Data
[VERIFY per provider before engagement; see `subprocessor-list.md`].
2.4 No voiceprint, face recognition or emotion-inference processing is performed. Any change requires
Customer's prior written agreement and an updated Schedule 1.

## 3. Customer responsibilities (recording, consent, AI disclosure)
3.1 Customer is responsible for having a lawful basis for, and for providing all notices and obtaining all
consents required for, recording, transcribing and analysing calls and meetings, including consent of
every participant where the law of any participant's location requires it (all-party consent and
wiretap laws, works-council or employee-information duties, and rules on special-category data).
3.2 The service displays an AI-participant/recording disclosure. Customer shall not disable, obscure or
circumvent it, and shall comply with the Acceptable Use Policy (`../acceptable-use-policy.md`) and consent
notice requirements (`../ai-transparency-and-meeting-consent.md`).
3.3 Customer shall not submit special-category data, children's data, or data subject to sector rules
(for example HIPAA, PCI DSS) unless Tavieri has agreed in writing and Schedule 1 is amended.

## 4. Confidentiality and personnel
Tavieri ensures that persons authorised to process Customer Personal Data are bound by written
confidentiality obligations, have least-privilege, role-based and audited access, and process the data
only as instructed.

## 5. Security
Tavieri implements and maintains the technical and organisational measures in **Annex II** (Art. 32 GDPR),
including tenant isolation, encryption in transit and at rest, secrets held only in a vault, hash-chained
audit logging, policy enforcement outside the model, and tested backup and restore. Tavieri may update
measures provided overall protection is not materially reduced.

## 6. Sub-processors
6.1 **General written authorisation.** Customer authorises Tavieri to engage the sub-processors listed in
`subprocessor-list.md` as at the Effective date (those with status "engaged").
6.2 **Notice and objection.** Tavieri will give at least **30 days'** notice (email to the Customer admin
contact and update of the list) before adding or replacing a sub-processor. Customer may object on
reasonable data-protection grounds within 15 days. The Parties will seek a resolution in good faith; if
none is reached, Customer may terminate the affected service and receive a pro-rata refund of prepaid fees.
6.3 **Flow-down.** Tavieri imposes on each sub-processor written data-protection terms no less protective
than this DPA (including CCPA 11 CCR section 7051(b) and Law 25 section 18.3 terms) and remains liable
for their performance.
6.4 A vendor must not receive Customer Personal Data until it appears on the list as "engaged".

## 7. International transfers
7.1 Default hosting is the EEA (Frankfurt) [CONFIRM per `subprocessor-list.md`]. Customer Personal Data is
transferred outside the EEA, UK or Switzerland only (a) under an adequacy decision, including the
EU-US Data Privacy Framework for certified recipients; or (b) under the Standard Contractual Clauses.
Tavieri does not rely solely on the Data Privacy Framework (its validity is under appeal, Case C-703/25 P)
and maintains SCCs as fallback with each US recipient.
7.2 **EU SCCs.** Where Customer is established in the EEA and Tavieri (or its sub-processors) is a
non-EEA importer, the Parties incorporate by reference the Standard Contractual Clauses annexed to
Commission Implementing Decision (EU) 2021/914, completed as in **Annex I-III**: **Module Two**
(controller to processor) where Customer is controller; **Module Three** (processor to processor) where
Customer is itself a processor. Elections: Clause 7 docking clause included; Clause 9(a) Option 2
(general authorisation, 30 days' notice); Clause 11(a) optional independent dispute-resolution body not
used; Clause 13 and Annex I.C as stated; Clause 17 Option 1, law of [EU MEMBER STATE]; Clause 18(b)
courts of [EU MEMBER STATE]. [COUNSEL: if Tavieri is EEA-established, SCCs apply only to onward
transfers (Tavieri as exporter under Module Three to non-EEA sub-processors) and not to Customer ->
Tavieri. If Tavieri is outside the EEA but within GDPR Art. 3(2), the 2021 SCCs are not intended for that
case; confirm the Commission's status on additional SCCs.]
7.3 **UK.** For restricted transfers under the UK GDPR the Parties incorporate the **UK Addendum** (ICO
International Data Transfer Addendum to the EU SCCs, version B1.0, in force 21 March 2022), completed as in
**Annex IV** (Part 1 Tables) with Part 2 Mandatory Clauses incorporated unchanged. The ICO has announced
it will update the IDTA and Addendum during 2026 for the Data (Use and Access) Act 2025; until replaced,
the current versions apply, and a newer approved version applies automatically only if Annex IV so elects.
[VERIFY the current ICO version at signing.]
7.4 **Switzerland.** Where the FADP applies, references to GDPR in the SCCs include the FADP, the Swiss
FDPIC is the competent authority for Swiss data subjects, and "Member State" includes Switzerland for
data-subject rights. [COUNSEL: confirm if Swiss customers are in scope.]
7.5 **Precedence.** SCCs prevail over this DPA and the Principal Agreement on conflict. Government-access
requests: Tavieri will notify Customer (unless legally prohibited), challenge overbroad requests, and
disclose the minimum necessary (SCC Clause 15). Tavieri will assist with transfer impact assessments (SCC
Clause 14) and has no reason to believe local law prevents compliance [CONFIRM after assessment].

## 8. Data subject requests and assistance
8.1 Tavieri will promptly (within 5 business days) forward to Customer any request received from an
individual about Customer Personal Data and will not respond except to redirect, unless Customer
instructs otherwise. Tavieri provides self-service export, correction and deletion tools and will assist
Customer in responding to requests (GDPR Arts. 12-22; CCPA consumer requests; PIPEDA and Law 25 access,
rectification, portability and de-indexing rights).
8.2 Tavieri will reasonably assist Customer with security, breach notification, data protection impact
assessments, privacy impact assessments (including Law 25 section 17) and prior consultation (GDPR Arts.
32-36), taking into account the nature of processing. Reasonable costs for assistance beyond the standard
product may be charged at [RATE].

## 9. Personal data breach
9.1 Tavieri will notify Customer's security contact **without undue delay and in any event within 48
hours** [CONFIRM operational capability; GDPR Art. 33(2) requires "without undue delay"] after becoming
aware of a breach affecting Customer Personal Data, including Law 25 "confidentiality incidents" and
incidents that present a real risk of significant harm under PIPEDA.
9.2 The notice will describe, to the extent known: nature and categories and approximate numbers of
individuals and records; likely consequences; measures taken or proposed; and a contact point. Information
may be provided in phases. Tavieri will contain, investigate, remediate and keep a record of the incident.
9.3 Customer decides on notifications to authorities and individuals; Tavieri will not notify them or make
public statements naming Customer unless legally required.

## 10. Audit and verification
10.1 Tavieri will make available information necessary to demonstrate compliance, including: current
security documentation, penetration-test summaries, sub-processor contracts (commercial terms redacted)
and, when available, independent reports such as SOC 2 [PLANNED; none exists yet] or ISO 27001.
10.2 Customer (or an auditor bound by confidentiality, not a Tavieri competitor) may audit Tavieri's
compliance **once per 12 months**, and after a confirmed breach or when a supervisory authority requires,
on 30 days' notice, during business hours, without disrupting operations or accessing other customers'
data. Customer bears audit costs. Tavieri may satisfy an audit request by questionnaire or remote review
first. This does not limit audit rights under SCC Clause 8.9 or supervisory authorities.
10.3 Tavieri will notify Customer promptly if it determines it can no longer meet its obligations under
Applicable Data Protection Law or this DPA, and Customer may then take reasonable and appropriate steps
to stop and remediate unauthorised processing, including suspension of transfers.

## 11. Deletion and return
11.1 During the term Customer may delete content at any time; deletion is propagated to primary storage
immediately and to backups within [35] days [CONFIRM against backup design]. Recordings and transcripts
follow the retention period Customer configures (default [30] days) [CONFIRM with product].
11.2 On termination or expiry Tavieri will, at Customer's choice, return (export in a standard machine-
readable format, available for 30 days) and then delete all Customer Personal Data within [30] days after
that window, including by cryptographic erasure, and certify deletion on request. Retention is permitted
only where law requires it, in which case the data is isolated and protected under this DPA.
11.3 Under Law 25 section 18.3, Tavieri does not keep Customer Personal Data after the end of the
service except as stated in this clause.

## 12. CCPA and US state law service-provider terms
Where CCPA applies, Customer is the "business" and Tavieri the "service provider". Tavieri:
(a) will not sell or share Customer Personal Data;
(b) will process it only for the specific business purposes in Schedule 1 (providing the service:
capture, transcription, summarisation, action proposal and approved execution, security, support) and not
retain, use or disclose it for any other purpose, any other commercial purpose, or outside the direct
business relationship, including by combining it with data from other sources, except as the CCPA permits;
(c) will comply with the CCPA and provide the same level of privacy protection required of businesses, and
maintain reasonable security (Cal. Civ. Code section 1798.81.5);
(d) grants Customer the right to take reasonable and appropriate steps to ensure Tavieri uses the data
consistently with Customer's CCPA obligations (including the review rights in clause 10) and, on notice,
to stop and remediate unauthorised use (including proof that deleted data is no longer retained);
(e) will notify Customer if it can no longer meet its CCPA obligations (clause 10.3);
(f) will enable Customer to comply with consumer requests (clause 8), and
(g) will bind any sub-processor by written contract meeting 11 CCR section 7051 (clause 6.3).
Tavieri certifies that it understands and will comply with these restrictions. The same terms apply to
other US state laws requiring processor contracts (e.g. Virginia, Colorado, Connecticut, Texas) to the
extent applicable, with processing instructions and nature, purpose and duration as in Schedule 1.

## 13. Canadian terms
13.1 Customer remains accountable under PIPEDA (Principle 4.1.3) for personal information transferred for
processing; Tavieri provides a comparable level of protection by contract, and will use Customer Personal
Data only for the mandate in this DPA, protect it with safeguards appropriate to its sensitivity, and
cooperate with the Office of the Privacy Commissioner or provincial commissioners.
13.2 For information relating to individuals in Quebec: the service is entrusted to Tavieri by written
agreement as required by Law 25 section 18.3; use is limited to carrying out the mandate; Tavieri will
notify Customer without delay of any violation or attempted violation of confidentiality obligations or
any confidentiality incident; Customer may verify compliance; Tavieri will not communicate the data
outside Quebec or to additional sub-processors except as stated in clauses 6 and 7; and will inform
Customer of any foreign legal requirement that may affect compliance. Customer is responsible for the
privacy impact assessment under section 17 and Tavieri will provide the information needed for it.
[COUNSEL: verify section 18.3 text and Quebec French-language requirements (Charter of the French
Language) for adhesion contracts and customer-facing documents.]

## 14. Liability, term, precedence
14.1 Each Party's liability under this DPA is subject to the limitations in the Principal Agreement, except
that nothing limits liability that cannot be limited by law or the rights of data subjects under SCC
Clause 12. [COUNSEL: confirm the cap and super-cap approach.]
14.2 This DPA lasts as long as Tavieri processes Customer Personal Data. It takes precedence over
conflicting terms of the Principal Agreement on data protection. Governing law and venue (subject to the
SCCs and mandatory law): [GOVERNING LAW / VENUE].

## Signatures
| For [LEGAL ENTITY NAME] (Tavieri) | For Customer: [CUSTOMER LEGAL NAME] |
|---|---|
| Name: [SIGNATORY NAME/TITLE] | Name: ______________________ |
| Signature: __________________ | Signature: __________________ |
| Date: [DATE] | Date: ______________________ |
| Address: [ADDRESS] | Address: ____________________ |
| Privacy contact: [EMAIL] | Customer privacy / security contact: ________ |

Signature of this DPA includes signature of the SCCs and UK Addendum incorporated above, with Annexes I-IV.

---
## Schedule 1 - Details of processing
| Item | Description |
|---|---|
| Subject matter | Provision of the Tavieri AI teammate service |
| Duration | Term of the Principal Agreement plus clause 11 |
| Nature | Collection, storage, transcription, summarisation, analysis by AI models, tool execution after human approval, deletion |
| Purposes (CCPA business purposes) | Providing the contracted service; security and fraud prevention; debugging; support; audit logging |
| Data types | Audio/video of calls, transcripts, summaries, names, emails, voice content, task and tool data; account identifiers. No special-category data (clause 3.3) |
| Data subjects | Customer personnel, and its clients and other call participants |
| Frequency | Continuous, per use |
| Retention | Per clause 11 |

## Annex I (SCC) - Parties, description of transfer, competent authority
**A. Parties.** Data exporter: Customer [NAME, ADDRESS, CONTACT, DPO/EU REPRESENTATIVE, ROLE]. Data
importer: [LEGAL ENTITY NAME], [ADDRESS], [CONTACT, DPO/EU REPRESENTATIVE], role processor. Signature and
date: as in the signature block above.
**B. Transfer.** Categories of data subjects, personal data, sensitive data (none), frequency, nature,
purpose, retention, and sub-processor transfers: as Schedule 1 and `subprocessor-list.md`. [COMPLETE]
**C. Competent supervisory authority (Clause 13):** [AUTHORITY, per exporter's establishment].

## Annex II (SCC) - Technical and organisational measures
[COMPLETE FROM `docs/security/` threat model and controls.] Minimum: encryption in transit (TLS 1.2+) and
at rest; tenant isolation in code and database row-level security; secrets in a vault only; SSO/MFA for
staff, least-privilege and audited access; append-only hash-chained audit; policy engine outside the
model with human approval gates; SSRF-safe egress and disposable browser sandboxes; vulnerability and
dependency management; logging without content or secrets; encrypted backups with tested restore;
retention limits and crypto-shredding; incident response plan; sub-processor due diligence; staff
training and confidentiality.

## Annex III (SCC) - Sub-processors
General authorisation (Option 2). Agreed list: `subprocessor-list.md` at signature date, filtered to status
"engaged" [ATTACH SNAPSHOT AT SIGNING].

## Annex IV - UK Addendum Part 1 (Tables)
Table 1 Parties: as Annex I.A (start date [DATE]). Table 2 Selected SCCs: the Approved EU SCCs
(Modules Two and Three, Clause 7 docking included, Clause 9(a) Option 2, 30 days). Table 3 Appendix
information: Annexes I-III as above. Table 4 ending when the Approved Addendum changes: [Importer / Exporter
/ neither]. Part 2 Mandatory Clauses incorporated unchanged. Governing law: England and Wales.
