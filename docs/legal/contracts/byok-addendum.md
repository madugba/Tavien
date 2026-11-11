> **DRAFT - NOT FOR SIGNATURE.** Prepared by Hannah Weiss (AI security architect) on 2026-11-11.
> Not legal advice. May be offered as a click-through addendum once counsel approves; applies only after
> the BYOK feature ships (planned for M3 in the design spec).

# Bring-Your-Own-Key (BYOK) Addendum

**Effective date:** [DATE] (or date of enabling a key)

Between **[LEGAL ENTITY NAME]**, [ADDRESS] ("**Tavieri**") and the customer or user enabling BYOK
("**Customer**"). It supplements the Terms of Service (`../terms-of-service.md`) or the Design Partner
Agreement (`design-partner-agreement.md`) and the Data Processing Agreement
(`data-processing-agreement.md`) (together the "**Principal Agreement**"). On conflict about keys and
provider charges this addendum prevails.

## 1. What BYOK is
Customer supplies an API key ("**Key**") for a third-party AI model provider ("**Provider**", for example
Anthropic, OpenAI or another compatible service) so that Tavieri sends Customer's model requests to the
Provider under Customer's account instead of Tavieri's. Tavieri does not resell Provider services, adds no
markup to Provider usage, and may still charge its own fees for the Tavieri service.

## 2. Customer's authority and Provider terms
Customer warrants that it (a) has authority to use and share the Key (for an organisation, the person
adding it is an authorised admin); (b) holds the Provider account lawfully and in good standing; (c) will
comply with the Provider's terms and usage policies, which govern the model service and are agreements
between Customer and Provider only; (d) has the right to send the content it submits to that Provider,
including any personal data and recorded content, and has made the notices and consents needed (including
recording-consent duties under the Principal Agreement); and (e) will not supply a Key belonging to
someone else or a Key obtained in breach of Provider terms. Tavieri does not verify or guarantee Key
validity, Provider availability, model behaviour or Provider terms.

## 3. Storage and handling of Keys
3.1 Tavieri stores each Key only in its secrets vault, encrypted at rest (envelope encryption, per-tenant
data keys), and never in prompts, logs, URLs, error reports, analytics, model context, process arguments or
backups outside the vault. After entry the Key is masked; only a short fingerprint is shown.
3.2 A Key is decrypted only inside the model gateway at call time, used solely to send Customer's
requests to the Provider for the workspace that owns it, and is never exposed to agents, tools, browser
sandboxes, other tenants, or Tavieri staff in plaintext except through audited break-glass access.
3.3 Tavieri will not use the Key for any other purpose, will not test it beyond a validation call when
added, and will keep access, rotations and uses in the tamper-evident audit log visible to Customer admins.
3.4 Customer should use a dedicated Key with provider-side spend limits and minimal scopes, and rotate it
regularly.

## 4. Revocation and removal
4.1 Customer may delete a Key at any time in the product. Tavieri stops using it immediately and deletes
the ciphertext from the vault within [24 hours] and from backups on the standard backup cycle [CONFIRM].
Customer should also revoke the Key at the Provider, which is the only way to guarantee it cannot be used.
4.2 Tavieri may suspend or disable a Key without notice if it suspects compromise, misuse, Provider
policy violation, or a legal or security need, and will tell Customer's admin promptly.
4.3 On termination of the Principal Agreement, Keys are deleted under the Principal Agreement's deletion
timetable (DPA clause 11).
4.4 If Customer suspects a Key is compromised, it must revoke it at the Provider and in Tavieri
immediately. Tavieri will notify Customer's security contact without undue delay and within [48] hours of
confirming that Tavieri's systems exposed a Key.

## 5. Charges and liability
5.1 **Customer is solely responsible for all Provider charges and quota consumption** incurred through a
Key, including usage from agent loops, long-running tasks, scheduled and background work, user error,
prompt injection, misconfiguration and unauthorised use not caused by Tavieri's breach of clause 3. Provider
bills Customer directly; Tavieri issues no credit for Provider charges.
5.2 Tavieri provides per-task and per-workspace budget caps and a kill switch that Customer should
configure. These are best-effort controls: usage reporting may lag Provider billing, so caps are not a
guarantee against charges, and the Provider's own spend limits remain the authoritative control.
5.3 If a Key is exposed because Tavieri breached clause 3, Tavieri will reimburse Provider charges that
Customer proves were caused by the exposure and incurred before Customer could reasonably revoke the Key,
subject to the liability cap in the Principal Agreement [COUNSEL: confirm cap, or a specific lower
sub-cap, for this exposure].
5.4 Tavieri is not liable for Provider outages, rate limits, price changes, content filtering, model
retirement, retention or training practices, or output quality; Tavieri's fallback (where Customer has
enabled it) to a Tavieri-supplied model is a billable choice by Customer and shown in the product.

## 6. Data and privacy position
6.1 Requests sent with a Key go to the Provider under Customer's own agreement. The Provider is
selected and contracted by Customer and is **not** a Tavieri sub-processor under `subprocessor-list.md`;
Customer is responsible for the Provider's data-processing terms, location, retention, and lawful
transfers. [COUNSEL: confirm this allocation; the alternative view treats the Provider as Customer's
processor to which Tavieri transmits data on instructions, and the DPA should then state that Tavieri's
responsibility ends at transmission.]
6.2 Tavieri will not enable Key use for a region or endpoint different from Customer's selection and will
display the Provider and region in use; the no-training commitment in DPA clause 2.3 applies to Tavieri
and Tavieri-supplied models, not to Providers chosen under BYOK.
6.3 Customer may restrict Providers, models and regions through admin policy; Tavieri enforces the
policy server-side outside the model.

## 7. Suspension and general
Tavieri may remove or limit BYOK on notice if a Provider changes terms in a way that makes it unlawful or
unsafe. Clauses 2, 4.4, 5 and 6 survive termination. Governing law and venue as in the Principal Agreement:
[GOVERNING LAW / VENUE].

## Signatures
| For [LEGAL ENTITY NAME] (Tavieri) | For Customer: [CUSTOMER LEGAL NAME] |
|---|---|
| Name/title: [SIGNATORY NAME/TITLE] | Name/title: ______________________ |
| Signature: __________________ | Signature: __________________ |
| Date: [DATE] | Date: ______________________ |

Click-through acceptance by an authorised admin when enabling the first Key is an acceptable alternative
form for self-serve customers, once counsel approves.

## Schedule - Enabled Providers
Provider: [NAME] | Region/endpoint: [REGION] | Key owner (admin): [NAME] | Spend limit set at Provider: [YES/NO]
