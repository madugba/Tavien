# ADR 0001 - Product name: Tavieri

- Status: accepted (2026-11-11), decided by the project lead under owner delegation
- Replaces: working name "Tavien"

## Context
"Tavien" was unavailable: a live site, tavien.ai (registered 2026-09-21), uses it for an AI assistant
product, and Tavus (AI avatars, funded) is a close, confusable mark in the same market (report 01).
The owner asked for a similar name that is available.

## Options considered
Candidates were screened for exact-match domains (.com, .ai, .app, .io via RDAP on 2026-11-11), package
names (npm, PyPI) and web presence. Unregistered everywhere: Tavieri, Taviella, Taviena, Taviyen, Tavienn.
Taviena was rejected as too close to Tavien/tavien.ai; Taviella reads as a personal name.

## Decision
**Tavieri** (working pronunciation TAH-vee-AIR-ee). Keeps the "Tavi-" cadence of the original, is
distinct from Tavus and tavien.ai, and has no web presence found for the exact name.

## Consequences
- Not trademark-cleared. Domain/handle checks are not a legal search. Counsel must search EUIPO, USPTO,
  CIPO, UKIPO and WIPO (Nice classes 9, 35, 42) before launch spend.
- Owner action: register tavieri.com (and optionally tavieri.app) now, at registrar cost. GitHub user/org
  availability could not be verified from this environment; the repository itself is still named
  `Tavien` and can be renamed in GitHub settings (redirects are automatic).
- Historical research reports in `docs/research/` keep the old name; they are dated records.
