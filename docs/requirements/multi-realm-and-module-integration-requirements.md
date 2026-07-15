# Multi-Realm and Module Integration Requirements

Status: **Authoritative S0 requirements**  
Current lifecycle status: **S0 — Proposed**

## Multi-realm requirements

- **MR-001:** The architecture shall support multiple AzerothCore server instances for distinct purposes.
- **MR-002:** Where technically feasible and validated, all client-compatible intended realms shall be visible through one shared login endpoint and realm-selection menu.
- **MR-003:** Shared authentication service and authentication database use shall be evaluated; worldserver realm state shall remain isolated.
- **MR-004:** Every realm shall have unique realm identity, name, port allocation, configuration, character database, world database, manifest, backup set, and lifecycle evidence unless a later ADR explicitly authorizes and validates an exception.
- **MR-005:** Realm labels shall identify gameplay experience or mod identity.
- **MR-006:** Baseline, research, development, integrated, and disposable classifications shall be supported.
- **MR-007:** Defined realms may remain stopped; simultaneous operation shall respect later-validated Steam Deck resource limits.
- **MR-008:** Client-modifying modules shall declare whether they require unified client assets, separate client profiles, or separate client installations.
- **MR-009:** Authorized remote players shall be supportable through separately governed public gameplay ingress; exact service ports and bind settings shall come from pinned configuration and approved deployment manifests.
- **MR-010:** Each networked service shall receive exactly one approved exposure classification per deployment profile, and public gameplay classification shall not make private or management services public.

## Version and convergence requirements

- **VC-001:** Each third-party module shall first be evaluated against its maintainer-documented reference baseline where reasonably possible.
- **VC-002:** Maintainer recommendations shall be distinguished from project verification.
- **VC-003:** Every reproducible manifest shall pin exact AzerothCore and module commits plus dependency, database, configuration, toolchain, and client state.
- **VC-004:** `latest`, `master`, `main`, and `current` shall not serve as final version evidence.
- **VC-005:** The Blaine integration baseline shall be selected through documented compatibility convergence across every desired feature and module.
- **VC-006:** The newest AzerothCore version shall not be automatic; an older baseline may be selected with evidence-backed justification.
- **VC-007:** Unsupported or abandoned modules shall trigger evaluation of porting, replacement, removal, or independent reimplementation rather than silently fixing the permanent baseline.
- **VC-008:** Cross-version databases shall require migration and rollback validation before interchange.

## Module and suite requirements

- **MS-001:** Provenance, authorship, license, notices, source pins, and modification history shall be preserved.
- **MS-002:** Candidates shall be classified as original, independently inspired, derived, dependency, or reference-only.
- **MS-003:** The player-facing Blaine experience may be distributed as a governed suite of separate modules; a monolithic source module is not required.
- **MS-004:** Suite components shall use documented interfaces, configuration conventions, dependency ordering, and a combined integration manifest.
- **MS-005:** Components shall remain independently buildable and testable where technically appropriate.
- **MS-006:** Individual success shall not approve integrated use.
- **MS-007:** Promotion shall require combined build, database, runtime, gameplay, upgrade, rollback, removal, security, stability, regression, and conflict validation.
- **MS-008:** Every exception shall have a decision record.

## Platform and lifecycle requirements

- **PL-001:** Linux and Windows shall remain separate implementation and validation scopes.
- **PL-002:** Every instance shall expose its exact platform-specific source/module manifest.
- **PL-003:** These requirements do not advance the project beyond **S0 — Proposed**.

## Control-plane, addon, and feature-package requirements

- **CP-001:** A proposed blAIne Realm Control plane shall remain separate from worldservers and capable of observing a stopped realm.
- **CP-002:** Initial Realm Control operation shall be `NET-HOST-LOCAL` and read-only; future remote access requires separate authenticated, encrypted, brokered, and auditable approval under ADR-0012.
- **FP-001:** Client addons shall be governed as a distinct asset class and treated as untrusted request origins.
- **FP-002:** A coordinated server-module/addon/control-plane feature shall pin and validate every component and protocol independently.
- **FP-003:** Protected gameplay state shall remain server-authoritative.
- **FP-004:** Future addon inventory and acquisition shall require human approval, provenance, licensing, exact version evidence, and disposition.
