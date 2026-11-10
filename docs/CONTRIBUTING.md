# Contributing to Tavien

How the Tavien team works on GitHub. Every issue, PR and review is signed by the role that
owns it, so history reads like a professional engineering organisation.

## Team roster

| Name | Role (agent) | Signs as |
|---|---|---|
| Amara Okonkwo | Product Owner (`product-owner`) | `[Amara]` |
| Emmanuel Adeyemi | Principal Engineer (`principal-engineer`) | `[Emmanuel]` |
| Lukas Brandt | Backend Engineer (`backend-engineer`) | `[Lukas]` |
| Priya Raman | Frontend Engineer (`frontend-engineer`) | `[Priya]` |
| Mateo Alvarez | Real-time/Media Engineer (`realtime-engineer`) | `[Mateo]` |
| Sofia Lindqvist | AI Systems Engineer (`ai-engineer`) | `[Sofia]` |
| Hannah Weiss | Security & Privacy Architect (`security-architect`) | `[Hannah]` |
| Kwame Mensah | QA Lead (`qa-lead`) | `[Kwame]` |
| Ravi Kapoor | DevOps / SRE (`devops-sre`) | `[Ravi]` |
| Isabel Moreau | UX/UI Designer (`ux-designer`) | `[Isabel]` |
| Chloe Bennett | Technical Writer (`tech-writer`) | `[Chloe]` |
| Jonas Eriksson | Growth & Launch Lead (`growth-lead`) | `[Jonas]` |

These are AI team members. Names identify ownership and accountability; they never claim
to be human, and the PR footer states that the work is AI-generated.

## Titles

`[Name] type(scope): imperative summary` - under 72 characters, no trailing period.

- Types: `feat`, `fix`, `refactor`, `perf`, `test`, `docs`, `build`, `ci`, `chore`, `security`.
- Scope: the module or area, e.g. `registry`, `auth`, `voice`, `web`, `infra`.
- Examples:
  - `[Emmanuel] docs(adr): choose Postgres-backed durable job queue`
  - `[Lukas] feat(registry): add capability registry with per-tool policy checks`
  - `[Kwame] test(e2e): cover media-permission revocation flow`

Issues use the same shape, signed by the author role: `[Amara] feat(tasks): approval inbox`.
Commits follow Conventional Commits and carry a `Persona: <Full Name> (<role>)` trailer.

## What a good PR looks like

1. **One purpose.** One logical change; split anything unrelated. Prefer < 400 changed lines.
2. **Linked issue.** `Closes #n`. No work without a ticket and acceptance criteria.
3. **Explains why**, not just what: problem, approach, alternatives rejected.
4. **Proves it works.** Tests added first (TDD); commands and results pasted; screenshots for UI.
5. **States risk.** Blast radius, migrations, cost impact, security/privacy impact, rollback.
6. **Meets the gates.** Format, lint, types, tests, 100-lines-per-file, secret scan: all green.
7. **Reviewed by another role.** Principal Engineer on all code; Security on auth/data/tools/media;
   QA on behaviour changes; Writer on docs-impacting changes.
8. **Draft until ready**; squash-merge with a clean Conventional Commit message.

## Branches

`type/short-kebab-description` (e.g. `feat/capability-registry`). Cloud sessions may be pinned
to a designated branch; the PR title carries the persona either way.

## Decisions and status

Significant decisions are ADRs in `docs/adr/`. Feature status in `docs/STATUS.md` advances
only with commit, test or release evidence.
