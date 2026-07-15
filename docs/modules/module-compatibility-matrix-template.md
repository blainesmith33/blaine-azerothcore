# Module Compatibility Matrix Template

Status: **Authoritative reusable template**  
Current lifecycle status: **S0 — Proposed**

Copy this template for each evaluated feature or module. Replace every placeholder with evidence or `Unknown — investigation required`; do not infer missing upstream claims. Moving source references are not exact pins.

## Identity, purpose, and provenance

| Field | Recorded value |
|---|---|
| Internal evaluation identifier | `<MOD-EVAL-NNNN>` |
| Feature or module name | `<NAME>` |
| Intended gameplay purpose | `<TESTABLE PURPOSE>` |
| Classification | `<original / independently inspired / derived / dependency / reference-only>` |
| Upstream project name | `<PROJECT OR NOT APPLICABLE>` |
| Upstream repository | `<CANONICAL URL OR NOT APPLICABLE>` |
| Upstream author or organization | `<AUTHOR OR ORGANIZATION>` |
| License | `<SPDX IDENTIFIER, LICENSE NAME, AND EVIDENCE>` |
| Module version | `<IMMUTABLE VERSION OR UNKNOWN>` |
| Module branch | `<BRANCH AT EVALUATION>` |
| Module tag | `<TAG OR NONE>` |
| Module exact commit SHA | `<FULL SHA OR DOCUMENTED LIMITATION>` |
| Last known upstream validation date | `<YYYY-MM-DD PLUS SOURCE>` |

## Maintainer-recommended reference baseline

| Field | Recorded value |
|---|---|
| Maintainer-recommended AzerothCore version | `<VERSION OR UNKNOWN>` |
| Maintainer-recommended AzerothCore branch | `<BRANCH OR UNKNOWN>` |
| Maintainer-recommended AzerothCore tag | `<TAG OR NONE>` |
| Maintainer-recommended AzerothCore exact commit | `<FULL SHA WHEN AVAILABLE>` |
| Recommendation evidence and observation date | `<SOURCE; YYYY-MM-DD>` |
| Required compiler and toolchain | `<EXACT REQUIREMENTS>` |
| Required database revisions | `<AUTH / CHARACTERS / WORLD / MODULE REVISIONS>` |
| Required AzerothCore modules or libraries | `<EXACT DEPENDENCIES AND PINS>` |
| Required core patches | `<PATCH IDS, CHECKSUMS, OR NONE>` |
| Required configuration changes | `<KEYS AND SANITIZED VALUES>` |
| Required client modifications | `<ASSETS, PATCHES, VERSION, OR NONE>` |
| Realm-specific constraints | `<REALM, PORT, DATABASE, OR CLIENT CONSTRAINTS>` |

## Project validation

| Field | Recorded value |
|---|---|
| Project-tested AzerothCore exact commit | `<FULL SHA OR NOT YET TESTED>` |
| Project-tested module exact commit | `<FULL SHA OR NOT YET TESTED>` |
| Successful clean build status | `<NOT RUN / PASS / FAIL; RUN AND VAL REFERENCES>` |
| Successful runtime status | `<NOT RUN / PASS / FAIL; RUN AND VAL REFERENCES>` |
| Successful gameplay validation status | `<NOT RUN / PASS / FAIL; RUN AND VAL REFERENCES>` |
| Compatibility with other candidate modules | `<CANDIDATE IDS AND EVIDENCE>` |
| Known symbol, hook, SQL, configuration, port, or database conflicts | `<DETAILS AND EVIDENCE>` |
| Security or stability concerns | `<DETAILS, SEVERITY, AND EVIDENCE>` |
| Port-forward estimate | `<SCOPE, RISK, AND ESTIMATE BASIS>` |
| Independent-reimplementation feasibility | `<ASSESSMENT, CLEAN-ROOM BOUNDARY, AND EVIDENCE>` |
| Replacement alternatives | `<CANDIDATES AND TRADEOFFS>` |
| Recommended disposition | `<research / adapt / port / replace / independently implement / reject / defer / reference-only>` |
| Evidence references | `<ADR / RUN / VAL / PROVENANCE / SOURCE REFERENCES>` |
| Reviewer and approval status | `<REVIEWER; pending / approved / rejected; DATE>` |

## Notes

- “Recommended” is an upstream claim, not project verification.
- Individual PASS results do not authorize integration promotion.
- Linux and Windows results must be recorded separately.
