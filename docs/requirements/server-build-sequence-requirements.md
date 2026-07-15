# Server Build Sequence Requirements

Status: Authoritative  
Lifecycle: S0 — Proposed  
Decision authority: ADR-0006

- SBS-001: SRV-BASELINE-001 is first after architecture and environment approval.
- SBS-002: The baseline is clean and unmodded, with no custom modules, core modifications, or client modifications.
- SBS-003: The baseline remains the control and is not silently repurposed.
- SBS-004: Requested research servers are introduced one at a time.
- SBS-005: The user explicitly approves research-server order.
- SBS-006: Each research server is independently validated with exact core and module commits.
- SBS-007: Each records provenance, licensing, dependencies, client and database requirements, and build, runtime, and gameplay evidence.
- SBS-008: Research distinguishes behavior to reproduce, retain, change, and reject.
- SBS-009: Research success does not approve integration or select its baseline.
- SBS-010: Compatibility convergence follows required research evidence.
- SBS-011: The Blaine realm is a governed user-owned experience, not an unreviewed module collection.
- SBS-012: Blaine may remain a suite of separate attributable and testable modules.
- SBS-013: Integration requires combined build, database, runtime, gameplay, conflict, removal, rollback, upgrade, provenance, and approval evidence.
- SBS-014: All templates remain disabled and PLANNED_NOT_IMPLEMENTED at S0.
- SBS-015: Linux evidence does not validate Windows.
- SBS-016: The internet-access and network-trust-boundary architecture shall be recorded before Buildx installation or baseline acquisition.
- SBS-017: The clean baseline shall be built and validated host-locally before inbound-connectivity conditions are characterized or an ingress mechanism is selected.
- SBS-018: Public gameplay ingress implementation requires a later ADR based on connectivity and threat-model evidence, followed by independent internet validation and private-service non-exposure validation.
- SBS-019: Authenticated remote management shall follow its own architecture and threat-model approval and shall not be introduced as a side effect of public gameplay exposure.
- SBS-020: Native Linux AMD64 baseline builds shall use the checksum-verified, user-scoped Buildx version approved by ADR-0013 and the Docker-driver builder associated with the active rootless context.
- SBS-021: Extra builders, remote nodes, registry pushes, QEMU, `binfmt_misc`, privileged helpers, and multi-platform emulation require later explicit approval and validation.
- SBS-022: After Buildx validation, the next major operation shall acquire and pin the official AzerothCore source for the clean unmodified baseline; source acquisition and building remain separate governed evidence boundaries.
- SBS-023: `SRC-ACORE-WOTLK-001` source objects are pinned to the acquisition-time official `master` commit, excluded from the governance repository, and shall not move with upstream without a new governed source update.
- SBS-024: A baseline image build requires both Git object integrity and a faithful detached working-tree checkout; object acquisition alone is not build readiness.
- SBS-025: Because the authoritative exFAT source path cannot represent the pinned symbolic link, a later decision shall approve a symlink-faithful working-tree location or mechanism and validate it before any AzerothCore image build.
- SBS-026: Database initialization, client-data acquisition, authserver startup, and worldserver startup remain separate post-build operations and shall not occur as a side effect of source acquisition or checkout remediation.
