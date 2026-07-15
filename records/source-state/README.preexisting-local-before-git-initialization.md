# AzerothCore on Steam Deck

Lifecycle status: **S0 — Proposed**

This repository-shaped project area governs a planned, reproducible AzerothCore implementation for Steam Deck Linux while reserving a distinct Windows implementation path. At S0, only the directory structure and governance documents exist. Docker, AzerothCore, databases, services, game-client files, and a local realm are not installed or operational.

## Authoritative project root

`/run/media/deck/blAIneXPLAT/azerothcore/`

Markdown files are the authoritative documentation format. Generated PDFs and other exports are derivative and cannot override their Markdown sources.

## Platform paths

- `linux/` is the active implementation path for future Steam Deck Linux work.
- `windows/` is reserved for an independently implemented and validated Windows path.
- Linux evidence does not validate Windows behavior, and Windows evidence does not validate Linux behavior.
- Common governance, requirements, architecture, decisions, audits, validations, releases, and public-release material remain at the project root.

## Governance and evidence

- [GOVERNANCE.md](GOVERNANCE.md) defines project-wide authority and control rules.
- [Project charter](docs/governance/project-charter.md) defines purpose, scope, approval, and evidence expectations.
- [Authority and versioning rules](docs/governance/authority-and-versioning-rules.md) define authoritative artifacts and supersession.
- [Change control](docs/governance/change-control.md) defines the controlled change workflow.
- [Public-release governance](docs/governance/public-release-governance.md) controls downstream public claims.
- [Acceptance criteria](docs/requirements/acceptance-criteria.md) define lifecycle stages S0 through S8.
- `records/` preserves decisions, audits, executions, validations, incidents, and releases.

Every technical claim must be supported by recorded commands, observed system state, validation evidence, or an explicitly documented source. Important architecture choices require an ADR identifier; validation activities require a VAL identifier. Modifying commands used in future Steam Deck work must be preserved in execution records.

## S0 amendment: multi-realm and module integration

The project now governs a future multi-instance topology and a modular Blaine gameplay suite. This is planning authority only; no server instance, module, shared authentication service, realm menu, database, or client modification has been implemented.

- [Multi-realm architecture](docs/architecture/multi-realm-architecture.md) defines isolated worldserver realms and the intended shared realm-menu experience where feasible.
- [Module-suite architecture](docs/architecture/module-suite-architecture.md) separates a unified player experience from a required monolithic source layout.
- [Module version and integration policy](docs/governance/module-version-and-integration-policy.md) requires exact pins and compatibility convergence.
- [Provenance and license policy](docs/governance/module-provenance-and-license-policy.md) governs third-party and original work.
- [Module evaluation](docs/modules/module-evaluation-procedure.md), [compatibility convergence](docs/modules/compatibility-convergence-procedure.md), and [integration promotion](docs/modules/integration-promotion-procedure.md) define future evidence gates.
- [Instance classification](docs/instances/instance-classification.md) defines baseline, research, development, integrated, and disposable scopes.

## Confidentiality

Secrets, passwords, private keys, account tokens, non-public IP addresses, and personal data must not enter public documentation. Publishable configuration examples must use sanitized placeholders.

## S0 amendment: temporary staging and server progression

The workspace at /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/ is temporary and non-authoritative; governance remains on blAIneXPLAT. Its permanent runtime destination is not selected, it contains no operational server or acquired source, and historical USB/UAS concerns prohibit treating it as the only copy of unique data.

Future server work follows ADR-0006: clean unmodded baseline first, user-ordered research servers one at a time, requirements extraction, then compatibility convergence before integrated Blaine evaluation. The baseline remains the control realm.

## Current boundary

The only completed work represented here is creation and validation of governed project structures and S0 governance documents. No implementation or later lifecycle stage is implied.

## blAIne component relationship

blAIne remains the broader umbrella orchestration and AI-systems-development identity. This AzerothCore development project and the provisional blAIne Realm Control component are subordinate to blAIne. Realm Control and full-stack feature packages are proposed architecture only: client addons are a distinct governed asset class, and coordinated module/addon/control-plane work requires explicit protocol, provenance, license, compatibility, and validation evidence.

The project-specific service boundary permits original tooling and integration services but excludes hosted game realms, sale of realm access, and distribution of proprietary game content. It does not redefine blAIne as a whole.
