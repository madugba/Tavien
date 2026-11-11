> **DRAFT - NOT LEGAL ADVICE - NOT FOR PUBLICATION.** Prepared 2026-11-11 by Hannah Weiss (security-architect, AI team member) for review and finalisation by qualified counsel. Bracketed items `[LIKE THIS]` are open decisions; see `README.md` checklist.

# Tavieri Cookie Policy

**Effective date:** [EFFECTIVE DATE]. Operator: **[LEGAL ENTITY NAME]**, [ADDRESS]. Contact: [DPO or PRIVACY CONTACT EMAIL].

## 1. What this covers
"Cookies" here includes similar technologies (local storage, session storage, pixels, SDKs) that store or read information on your device. This policy complements the Privacy Policy.

## 2. Our approach
- **Strictly necessary:** always on, because the service cannot work without them. No consent is required.
- **Analytics:** **off until you opt in.** Choosing "Reject" or ignoring the banner keeps them off, and rejecting is as easy as accepting.
- **No advertising or cross-site tracking cookies.** We do not sell or share data for behavioural advertising.

## 3. Cookies we use
| Name / type | Purpose | Category | Duration | Provider |
|---|---|---|---|---|
| [session / auth cookie] | Keep you signed in; protect sessions | Strictly necessary | Session / [X] days | Us / [AUTH PROVIDER] |
| [CSRF token] | Prevent cross-site request forgery | Strictly necessary | Session | Us |
| [consent-choice cookie] | Remember your cookie choice | Strictly necessary | [6-12] months | Us |
| [load balancing / region] | Route requests, security, fraud prevention | Strictly necessary | Session | [HOSTING PROVIDER] |
| [analytics identifier] | Understand feature use to improve the product | Analytics (opt-in) | [X] months | [ANALYTICS PROVIDER] |

[ENGINEERING: replace with an audited inventory from a real scan before publication; update on every release that adds a cookie or SDK.]

## 4. Managing your choices
Use **Cookie settings** (footer link) to accept or withdraw analytics consent at any time. Withdrawing is effective immediately for future data. We also honour **Global Privacy Control** and "Do Not Track"-style opt-out signals as a rejection of analytics [COUNSEL: confirm]. You can block or delete cookies in your browser, but strictly necessary ones are needed to sign in.

## 5. Legal bases
EU/UK: strictly necessary cookies fall within the exemption in the ePrivacy Directive and UK PECR; analytics rely on your consent (and on any narrower PECR exemptions only if counsel confirms they apply to our setup). Personal data collected via cookies is handled as described in the Privacy Policy.

## 6. Third-party cookies
Analytics providers may set their own cookies only after you opt in. Their privacy notices: [LINKS]. Data may be transferred outside your country as explained in the Privacy Policy.

## 7. Changes
We will update this policy and, where the law requires, ask for fresh consent when we add new non-essential cookies.
