# Project Governance

Status: **Authoritative**  
Lifecycle applicability: **S0 through S8**  
Current lifecycle status: **S0 — Proposed**

## Governing principles

1. Markdown is the authoritative documentation format.
2. Each scope and artifact class has exactly one authoritative current version.
3. A superseded authoritative file is moved to its defined archive location; it is not left beside the current file.
4. Public documentation is downstream from governance, architecture, procedure, execution, and validation evidence.
5. A public document may not claim a procedure works until the corresponding implementation was executed and validated.
6. Linux and Windows are separate implementation paths. Success on one does not validate the other.
7. Commands that modify the Steam Deck must be captured in an execution record.
8. Important architecture decisions receive sequential `ADR-NNNN` identifiers.
9. Validation activities receive sequential `VAL-NNNN` identifiers.
10. Failed operations and materially unexpected behavior are retained in execution or incident records.
11. Secrets and personal or non-public operational data are excluded from public documentation; examples use sanitized placeholders.
12. PDFs and other generated exports are derivative artifacts and never authoritative over Markdown.

## S0 amendment: multi-realm and module governance

- Multiple server instances are governed as separately identified and evidenced scopes, even when they later share authentication or one realm-selection menu.
- Module maintainer recommendations are research inputs, not project verification or automatic Blaine integration baselines.
- Reproducible instance and suite manifests require exact core, module, dependency, database, configuration, toolchain, and client identifiers; moving references are insufficient.
- Third-party provenance, authorship, licensing, notices, and modification history are retained through evaluation, rejection, porting, replacement, reimplementation, and release.
- A unified Blaine player experience may be implemented as a governed suite of separate modules. Individual success does not constitute combined integration approval.
- Linux and Windows module, instance, database, client, and integration evidence remains platform-specific.

## Governance hierarchy

Authority descends in this order:

1. Human-approved project charter and governance rules.
2. Human-approved requirements and architecture decisions.
3. Platform-specific procedures and controlled configuration.
4. Execution records describing what was actually done.
5. Validation records describing what was observed and accepted.
6. Public documentation and derivative exports.

Downstream artifacts must cite or identify the upstream authority and evidence on which their claims depend. When artifacts conflict, the higher authoritative layer controls until the conflict is resolved through change control.

## Temporary staging and progression authority

The temporary staging root is implementation preparation only and cannot supersede governance or evidence here. Paths are centralized, secrets and live runtime state are prohibited, migration requires checksums, and staging may not hold the only copy of unique data.

Server promotion follows ADR-0006. The clean baseline remains control; research order requires user approval; integrated promotion is gated by independently validated research and compatibility convergence.

## Human approval and Codex execution

The human project owner authorizes scope, risk-bearing changes, architecture approval, lifecycle advancement, and release decisions. Codex may execute only the authorized scope, record commands and results, surface failures, and prepare evidence. Codex execution does not itself constitute human approval or lifecycle advancement.

## Records

- `records/decisions/`: ADRs and other approved decisions.
- `records/audits/`: observed baseline and compliance audits.
- `records/executions/`: command-level execution records (`RUN-NNNN`).
- `records/validations/`: validation records (`VAL-NNNN`).
- `records/incidents/`: failures or unexpected behavior requiring dedicated follow-up.
- `records/releases/`: approved release evidence and manifests.
- `records/modules/`: module provenance, compatibility, build, test, and release evidence.

Records are append-oriented evidence. Corrections must preserve the original fact pattern, explain the correction, and identify authorizing evidence.

## Lifecycle control

The acceptance criteria define stages S0 through S8. Advancement requires the stated evidence, completed validation, and explicit human approval. This project remains at **S0 — Proposed** until those conditions are recorded.

## Realm Control, addons, and feature packages

blAIne Realm Control is governed as a subordinate, separate control-plane component. Client addons are a distinct acquired-or-original asset class. Full-stack feature packages may coordinate modules, addons, Realm Control definitions, protocols, presets, migrations, configuration, documentation, and tests, but each component retains independent versions, provenance, licensing, compatibility, and validation.

Future addon inventory or acquisition requires explicit human approval. Upstream work cannot become original blAIne work through renaming. The non-hosting and service boundary in ADR-0009 is specific to this AzerothCore project.
