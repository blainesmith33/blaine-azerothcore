# RUN-0012 — Record Internet Access and Network-Boundary Architecture

## Record control

- Date: 2026-07-14 (America/Phoenix)
- Lifecycle: `S0 — Proposed`
- Objective: record required future internet gameplay access, separate public gameplay from authenticated management and private services, align current architecture and README language, validate, commit, and push normally to `main`.
- Authorized root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Remote: authorized `origin` verified against the operation request
- Branch: `main`
- Starting commit: `22083a6d9a598dfa27d17be8518fee2770399dc8`
- Publication state in this revision: pre-commit documentation and governance evidence prepared; push verification pending.

## Authorization boundary

This operation is documentation, architecture, governance, Git commit, and normal push only. It authorizes no Docker, Buildx, service, container, firewall, router, port-forwarding, dynamic-DNS, VPN, overlay, tunnel, proxy, relay, cloud, SSH, listener, LAN test, internet test, AzerothCore, ChromieCraft, client, module, or documentation-corpus action. No lifecycle advancement is authorized.

## Starting repository and remote state

- Canonical root: `/run/media/deck/blAIneXPLAT/azerothcore`.
- Filesystem: `/dev/sda1`, exFAT, mounted read/write.
- Git repository: yes; active branch `main`; no active merge, rebase, cherry-pick, revert, bisect, or sequencer operation.
- Working tree: clean.
- Origin: exact authorized SSH remote.
- Local HEAD, local `origin/main`, and remote `refs/heads/main`: all `22083a6d9a598dfa27d17be8518fee2770399dc8`.
- Tracked-file count: 78.
- Recent history: `22083a6 Record blAIne AzerothCore publication validation`; `eabf64e Publish governed blAIne AzerothCore project scaffold`; `950f6bf Create README.md`.
- Lifecycle references in README and GOVERNANCE: `S0 — Proposed`.

## Documents inspected

- `README.md`
- `docs/governance/project-charter.md`
- `docs/architecture/multi-realm-architecture.md`
- `docs/architecture/blaine-realm-control-architecture.md`
- `docs/architecture/command-broker-and-execution-model.md`
- `docs/architecture/client-addon-server-module-contract.md`
- `docs/architecture/server-progression-and-promotion-architecture.md`
- `docs/requirements/azerothcore-project-service-boundary.md`
- `docs/requirements/server-build-sequence-requirements.md`
- `docs/requirements/realm-control-requirements.md`
- `docs/requirements/multi-realm-and-module-integration-requirements.md`
- `records/decisions/ADR-0002-multi-realm-server-architecture.md`
- `records/decisions/ADR-0007-blaine-realm-control-architecture.md`
- `records/decisions/ADR-0009-azerothcore-project-service-and-hosting-boundary.md`
- `records/decisions/ADR-0010-rootless-docker-runtime-persistence-storage-and-network-architecture.md`
- `records/decisions/ADR-0011-rootless-docker-fuse-overlayfs-storage-backend-remediation.md`

## Conflicting assumptions found and corrected

- Current README and Realm Control documents used broad local-only language without distinguishing the initial host-local profile from a future authenticated remote-management plane.
- README wording allowed the unresolved rootless `pasta` loopback listener-shape result to appear broader than its host-local validation scope.
- Current multi-realm and sequencing documents did not yet require per-profile exposure classification or place ingress selection after the clean host-local baseline.
- Historical ADR-0007 and Docker RUN/VAL records were preserved unchanged. ADR-0012 records the later clarification and relationship without rewriting historical evidence.
- No document authorized Realm Control to expose Docker or databases directly; the existing broker boundary was reinforced, not reversed.

## Documents created

- `docs/architecture/internet-ingress-and-network-trust-boundary-architecture.md`
- `records/decisions/ADR-0012-public-game-access-and-private-management-network-boundaries.md`
- `records/executions/RUN-0012-record-internet-access-and-network-boundary-architecture.md`
- `records/validations/VAL-0012-internet-access-and-network-boundary-architecture-validation.md`

## Documents updated

- `README.md`
- `docs/governance/project-charter.md`
- `docs/architecture/blaine-realm-control-architecture.md`
- `docs/architecture/multi-realm-architecture.md`
- `docs/architecture/command-broker-and-execution-model.md`
- `docs/requirements/realm-control-requirements.md`
- `docs/requirements/multi-realm-and-module-integration-requirements.md`
- `docs/requirements/server-build-sequence-requirements.md`

## Architecture decision summary

ADR-0012 accepts public gameplay with separately governed private and authenticated management boundaries while deferring the final ingress mechanism pending connectivity and threat-model evidence. It establishes `NET-HOST-LOCAL`, `NET-PRIVATE-INTERNAL`, `NET-LAN-RESTRICTED`, `NET-REMOTE-AUTHENTICATED`, `NET-PUBLIC-GAME`, `NET-PUBLIC-WEB`, and `NET-PROHIBITED-DIRECT`.

No exact public port, address, domain, provider, ingress product, or current inbound capability is approved or assumed.

## Actions explicitly not performed

No runtime or network configuration changed. No port, firewall rule, router setting, Docker setting, RootlessKit driver, VPN, tunnel, DNS, proxy, relay, SSH setting, service, container, listener, or external connectivity test was created or changed. No dependency or restricted asset was downloaded. No credential, private address, current network identifier, or secret was added.

## Staged audit, commit, and push

The pre-commit staged set contains exactly the 12 intended paths: four new architecture/governance records and eight narrowly updated current documents. `git status --short`, cached stat, cached name-status, cached whitespace check, and the complete cached diff were reviewed. The whitespace check passed.

Every staged object is mode 100644 Markdown detected as plain text. No staged object is executable, a symlink, binary, larger than 25 MiB, a restricted/client/database/runtime asset, or an unexpected path. Path-only and content scans found no suspected secret, credential, private address, current network identifier, account identifier, or domain literal. Port-specific review found no numeric socket, bind, or approved gameplay/management port. No vendor or product was selected, no prior RUN or VAL record was modified, and no lifecycle advancement was introduced.

Commit and push results remain pending in this pre-publication revision.

## Lifecycle statement

This operation records architecture only. No lifecycle advancement occurred; the project remains **S0 — Proposed**.
