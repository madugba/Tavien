> **DRAFT - NOT FOR SIGNATURE.** Prepared by Hannah Weiss (AI security architect) on 2026-11-11.
> Not legal advice. Requires counsel review in the contributor's country: copyright assignment, moral
> rights, remuneration and worker-status rules differ materially (see clause 4 and `README.md`).
> For **human** contributors and contractors only. AI agents have no legal capacity and cannot sign.

# Contributor and IP Assignment Agreement

**Effective date:** [DATE]

**Between** [LEGAL ENTITY NAME], [ADDRESS] ("**Tavieri**") **and** [CONTRIBUTOR NAME], [ADDRESS],
[COUNTRY OF RESIDENCE] ("**Contributor**").

## 1. Engagement and status
Contributor provides the services described in **Schedule 1** (code, designs, documentation, research,
prompts, evals, data, or other work) as an independent contractor or volunteer, not an employee, unless a
separate written employment agreement says otherwise (employees are covered by their employment contract,
not this document). Contributor controls how and when the work is done, supplies their own tools, and is
responsible for their own taxes. [COUNSEL: worker-classification check per country, for example IR35 in the
UK, Scheinselbststaendigkeit in Germany, ABC tests in California.]

## 2. Definitions
"**Work Product**": everything Contributor creates, alone or with others or with AI tools, in the course of
the engagement or using Tavieri resources, confidential information or repositories, including source code,
designs, text, data, models, prompts, evaluations, inventions and improvements, whether or not
registrable. "**Pre-existing IP**": Contributor's IP created before the engagement or independently of it,
listed in **Schedule 2**. "**AI-Assisted Work**": Work Product created in whole or part with generative AI
tools (clause 9).

## 3. Assignment
3.1 Contributor **hereby assigns** to Tavieri, with full title guarantee and effective on creation, all
right, title and interest worldwide in the Work Product, including copyright, database rights, patents,
design rights, trade secrets and all other intellectual property, with the right to sue for past
infringement. To the extent future assignment is invalid, Contributor agrees to assign on creation.
3.2 **Work made for hire.** To the extent US law treats any Work Product as a work made for hire, it is so
treated; otherwise clause 3.1 applies. (Independent contractors can be authors of works for hire only in
nine statutory categories, so the assignment is the primary mechanism.)
3.3 **Fallback licence.** Where law does not allow assignment of any right (for example copyright in some
civil-law countries such as Germany, where author's rights are inalienable), Contributor grants Tavieri an
exclusive (including as to Contributor), irrevocable, perpetual, worldwide, transferable, sublicensable,
royalty-free licence to use, modify, distribute and exploit that right for all known and unknown uses, to
the fullest extent permitted.
3.4 Contributor will sign further documents and do what is reasonably required to perfect, register and
enforce Tavieri's rights, at Tavieri's cost, and appoints Tavieri as attorney-in-fact solely to sign
such documents if Contributor does not within [10] days of request (where lawful).
3.5 **Pre-existing IP.** Contributor keeps it. If included in Work Product, Contributor grants Tavieri a
perpetual, irrevocable, worldwide, royalty-free, non-exclusive, transferable, sublicensable licence to use
and exploit it as part of the Work Product. Contributor will not include Pre-existing IP not listed in
Schedule 2.

## 4. Moral rights
To the maximum extent permitted by law, Contributor waives, and agrees not to assert, all moral rights
(attribution, integrity, anonymity, and the right to withdraw) against Tavieri, its licensees and successors.
Where waiver is not permitted, Contributor consents to uses that would otherwise infringe and agrees to
exercise those rights only in ways that do not impede Tavieri's exploitation. Jurisdiction notes
[COUNSEL]: **UK** rights are waivable in writing (CDPA 1988 s.87); **Canada** moral rights cannot be
assigned but can be waived (Copyright Act s.14.1); **Germany** and **France** moral rights are not
waivable, so a covenant not to assert within lawful limits is used; the **US** has narrow moral rights
(VARA, not for software). Attribution credit, if any, is given only as agreed in Schedule 1.

## 5. Consideration and remuneration
Consideration is the fees or benefits in Schedule 1 [or: the opportunity to contribute, if unpaid] and
other good and valuable consideration, acknowledged. Fees for the assignment are included in the
remuneration. [COUNSEL: EU authors may have a statutory right to appropriate remuneration (for example
Germany UrhG sections 32 and 32a, DSM Directive Arts. 18-20) that cannot be contracted out of.]

## 6. Warranties and obligations
Contributor warrants that to the best of their knowledge: (a) Work Product is their own original work, or
properly licensed under clause 7; (b) it does not infringe third-party IP or breach any duty (including
to a current or former employer, university or client); (c) Contributor is free to enter into this
agreement and has not used third-party confidential information or code under incompatible licences; and
(d) disclosures in Schedule 3 about AI tools are accurate. Contributor will promptly tell Tavieri about
any known claim or licence issue. Tavieri may reject, remove or rewrite any contribution.

## 7. Open-source policy
7.1 **Inbound.** Contributor may include third-party open-source code only if its licence is on the
approved list in **Schedule 2B** (permissive: MIT, BSD-2/3-Clause, ISC, Apache-2.0, Unlicense/CC0;
MPL-2.0 with prior approval) and the dependency is added through the project's manifests with a
pinned, verified version. **Prohibited without written approval**: AGPL, GPL, LGPL, SSPL, BUSL,
Commons Clause, "non-commercial" or no-licence code, and code copied from Q&A sites or unlicensed
repositories. Contributor records source, version and licence of any copied snippet in the commit.
7.2 **Outbound.** Tavieri alone decides whether any Work Product is published as open source, and under
what licence [LICENCE - TO BE DECIDED, ADR]. Contributor will not publish, push to public repositories or
submit to upstream projects any Work Product, or derivative of Tavieri code, without written approval.
7.3 **Upstream contributions** made on Tavieri's behalf are made under the upstream licence and project
terms (for example DCO sign-off), only with approval, and are not assigned to Tavieri to the extent that
conflicts with those terms.
7.4 **Supply chain.** No dependency from unverified sources or typosquats; lockfiles committed; security
advisories monitored; a software bill of materials is produced for releases.

## 8. Confidentiality, security and data
8.1 Contributor keeps non-public information of Tavieri, its customers and partners confidential during
and for [5] years after the engagement (trade secrets, for as long as protected), and will use it only for
the work. The Mutual NDA (`mutual-nda.md`) terms apply as a minimum; this clause governs on conflict.
8.2 Contributor never puts secrets (keys, tokens, passwords) in code, prompts, logs, URLs, issues or process
arguments; uses the vault; never copies customer data to personal devices or non-approved tools; and
reports suspected incidents to [SECURITY EMAIL] within 24 hours.
8.3 Contributor follows the repository's rules (`CLAUDE.md`, `docs/CONTRIBUTING.md`, security review gates).
8.4 Whistleblowing and protected disclosures to authorities are unaffected (DTSA 18 U.S.C. section 1833(b)
notice applies to US individuals).

## 9. AI-generated contributions and ownership uncertainty
9.1 **Disclosure and approved tools.** Contributor may use only the AI tools listed in **Schedule 3** or
later approved in writing, on accounts whose terms (a) assign or license outputs to the user for
commercial use, (b) do not train on inputs, and (c) allow the use contemplated. Contributor discloses AI
assistance in the commit or pull request (for example a `Co-Authored-By` trailer) and keeps a provenance
note for substantially AI-generated files.
9.2 **Human responsibility.** Contributor reviews, tests and takes responsibility for all AI-Assisted Work,
makes real human modifications and design choices where practicable, and remains liable under clause 6 for
it, including for infringement, licence contamination (memorised third-party or copyleft code),
security flaws and leaked confidential data in prompts. Tavieri may require licence and similarity scans.
9.3 **Assignment is of whatever rights exist.** Clause 3 assigns all rights Contributor has or acquires in
AI-Assisted Work, including rights in human selection, arrangement and modification, and any rights in
prompts and any rights arising under contract with the AI provider. Contributor does not warrant that
copyright subsists in purely AI-generated material.
9.4 **Acknowledged uncertainty (status as researched 2026-11-11; counsel to verify).**
 - **United States:** copyright requires human authorship. The Copyright Office's position (2023 guidance;
   2025 report on copyrightability) is that prompts alone do not make a user the author of AI output, while
   human-authored expression, and creative selection, coordination, arrangement or substantial
   modification of AI output, can be protected. *Thaler v. Perlmutter* (D.C. Cir. 2025) confirmed human
   authorship for an AI-generated image; commentators note that no court has yet applied this to AI-generated
   source code. Registration requires disclosing and disclaiming AI-generated material.
 - **EU:** works must be the "author's own intellectual creation" (CJEU), generally understood as requiring
   a human author; protection for AI-generated code is therefore doubtful.
 - **UK:** CDPA 1988 s.9(3) gives authorship of "computer-generated works" to the person making the
   arrangements, but the provision is under government review and its application to generative AI is
   untested [VERIFY current status].
 - **Canada:** authorship has been understood to require human skill and judgment; the position for AI-assisted
   works is unsettled.
 - Consequence: Tavieri may not be able to enforce copyright in purely AI-generated code and may need to
   rely on trade-secret protection, contracts, licences, access controls, and the human-authored parts. A
   large proportion of Tavieri's code is expected to be AI-generated (the engineering team includes AI
   agents), so provenance records matter commercially (investor, acquirer and customer due diligence).
9.5 **AI agents.** AI agents that contribute to the repository are tools of Tavieri, not authors or
assignors. The named human or Tavieri signatory who directs and approves their work is responsible for its
review and provenance entry. No claim by or on behalf of an AI agent is recognised.
9.6 **Provider terms.** Contributor will not use output in breach of provider terms (for example using it
to develop a competing model) and will tell Tavieri if a provider's terms change in a way that affects
ownership or indemnity.

## 10. Term and termination
This agreement applies from the Effective date to all Work Product created before and after, including
Work Product created before signature for Tavieri [CONFIRM RETROACTIVE SCOPE]. Either party may end the
engagement on [14] days' notice (or per the services contract); clauses 3, 4, 6, 7.2, 8, 9 and 11 survive.
On end, Contributor returns or deletes Tavieri materials and confirms in writing.

## 11. General
No obligation to use any contribution. Limitation of liability: Tavieri's liability is limited to fees
unpaid, to the extent permitted by law [COUNSEL]. Entire agreement on its subject; changes in writing;
electronic signatures valid; severability. Governing law and venue: [GOVERNING LAW / VENUE]; mandatory law
of Contributor's country (including copyright and consumer law) is unaffected.

## Signatures
| For [LEGAL ENTITY NAME] | Contributor |
|---|---|
| Name/title: [SIGNATORY NAME/TITLE] | Name: [CONTRIBUTOR NAME] |
| Signature: __________________ | Signature: __________________ |
| Date: [DATE] | Date: ______________________ |
| | Country of residence: ______________ |

## Schedule 1 - Services and contributions
Description, deliverables, fees and payment, attribution (if any), repositories and areas: [COMPLETE].

## Schedule 2 - Pre-existing IP / 2B Approved open-source licences
2: [LIST OR "NONE"]. 2B: see clause 7.1; additions need written approval from Tavieri's principal engineer
and recorded in `docs/adr/`.

## Schedule 3 - Approved AI tools and disclosure
Approved tools and account types: [e.g. Claude (commercial plan), OTHER]. Contributor's disclosure of AI
use to date: [LIST OR "NONE"].
