> **DRAFT - NOT FOR SIGNATURE.** Prepared by Hannah Weiss (AI security architect) on 2026-11-11.
> Not legal advice. No document here is ready to sign until qualified counsel has reviewed it and every
> `[PLACEHOLDER]` is filled. Only human signatories may sign; AI team members cannot bind Tavieri.

# Signing Guide

## Before anything is signed
1. Form the legal entity and fill `[LEGAL ENTITY NAME]`, `[ADDRESS]`, `[GOVERNING LAW / VENUE]`,
   `[SIGNATORY NAME/TITLE]` in every file (one search-and-replace pass; grep for `\[`).
2. Counsel reviews each document (questions below). The owner decides who the authorised signatory is.
3. Companion documents drafted separately (referenced by filename; confirm they exist and match):
   `../privacy-policy.md`, `../terms-of-service.md`, `../acceptable-use-policy.md`,
   `../cookie-policy.md`, `../ai-transparency-and-meeting-consent.md`.

## Who signs what, and in what order
| Step | Document | Tavieri signs | Counterparty signs | When |
|---|---|---|---|---|
| 1 | `mutual-nda.md` | Owner / [SIGNATORY] | Interviewee or prospect | Before sharing the product, roadmap or any non-public information |
| 2 | `contributor-and-ip-assignment.md` | Owner / [SIGNATORY] | Each human contributor or contractor | Before repository access or any paid work; also retroactively for past work |
| 3 | `design-partner-agreement.md` | Owner / [SIGNATORY] | Partner's authorised officer | After the NDA, before pilot access |
| 4 | `data-processing-agreement.md` (with SCC, UK Addendum, Annexes I-IV) | Owner / [SIGNATORY] | Partner/customer | Before any personal data is uploaded; the design partner agreement bars personal data until then |
| 5 | `subprocessor-list.md` | Not signed; snapshot attached as Annex III and Schedule 2 | Customer notified of changes | Only rows with status "engaged"; update before engaging any vendor |
| 6 | `byok-addendum.md` | Owner / [SIGNATORY], or click-through | Customer admin | Only when BYOK ships (M3) and a key is added |

Execution: electronic signature (eIDAS simple/advanced, ESIGN, UETA) is fine for all six; confirm with
counsel whether any local formality (for example written form for copyright assignment in some
countries, or wet ink for notarised items) is needed for contributors outside the US/UK/EU. Keep signed
copies and the executed subprocessor snapshot in [CONTRACT REPOSITORY] outside the public repository.

## Hard stops (security gates)
- No customer personal data to any vendor until it is "engaged" in `subprocessor-list.md` and a vendor
  DPA plus SCCs or an adequacy basis is signed. Today nothing is engaged.
- No-training and zero-retention commitments (DPA 2.3) must be confirmed per model provider first.
- Verify before signing: current ICO Addendum version (update promised during 2026); EU-US Data Privacy
  Framework status (valid, appeal C-703/25 P pending); CCPA 11 CCR section 7051 text; Quebec Law 25
  section 18.3 text.

## Questions for counsel (ranked)
1. **Entity and SCC structure.** Where is the entity established? If in the EEA, Customer -> Tavieri needs
   no SCC and Module Three applies to non-EEA sub-processors; if outside the EEA, does GDPR Art. 3(2) make the
   2021 SCCs unsuitable (new Commission SCCs pending)? Do we need an EU/UK Art. 27 representative?
2. **Recording consent and bots.** All-party consent laws (US states, Quebec), works councils and
   employee monitoring (DACH), disclosure of AI participants (EU AI Act Art. 50 applies since 2026-08-02).
   Is allocating this to the customer enough, and what product-side obligations remain?
3. **Controller/processor line.** Is Tavieri processor for summaries, action proposals and audit logs, and
   independent controller for product analytics? Is the BYOK provider a sub-processor or the customer's
   own processor? Does the 48-hour breach notice and the audit regime fit our capacity?
4. **Liability.** Caps, super-caps for data breach, indemnity for consent failures; unenforceable
   disclaimers under German/EU law for a free pilot (cardinal duties, gross negligence).
5. **Swiss and UK scope.** Add Swiss FADP adaptations now (DACH market)? Confirm the UK Addendum version
   and the DUAA transfer test.
6. **Canada.** Quebec Law 25 section 17 PIA allocation, section 18.3 contract content, French-language
   requirements for adhesion contracts and notices, PIPEDA comparable-protection wording.
7. **IP ownership of AI-written code.** The team that builds Tavieri includes AI agents. How do we
   document human authorship and provenance, protect code as trade secret, and describe ownership
   truthfully to investors and customers? Is the retroactive assignment in `contributor-and-ip-assignment.md`
   clause 10 effective, and are Germany/France remuneration and moral-rights rules handled?
8. **Open-source licence** for any Tavieri code we publish (decision pending; ADR to follow).
9. **Design partner terms.** Feedback licence, publicity consent, pilot-to-commercial conversion, and
   whether the pilot can be offered to individuals/sole traders (consumer law).
10. **Likeness and voice rights** and trademark risk (name collision with Tavus; tavieri.ai ownership) as
    flagged in the design spec.
