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
- SBS-027: Clean baseline builds shall use `CHK-ACORE-WOTLK-001` as the approved source checkout unless a later ADR explicitly replaces it.
- SBS-028: Build input shall remain detached at source pin `34a8bd6655c02448d3da6195dcd00647f634dde3`, match tree `25ad25f5fb8e119f68fef69b080934f1182ad8d6`, and have a clean working tree and index immediately before build.
- SBS-029: The approved checkout's `.git` shall remain a self-contained directory with copied objects and refs wherever the pinned build definition requires Git metadata; linked-worktree pointers, alternates, hardlinks, and portable-drive object dependencies are not approved.
- SBS-030: Core modifications, generated source changes, and modules require a separate governed source path and shall not mutate `CHK-ACORE-WOTLK-001`.
- SBS-031: Structural build-context validation does not authorize or validate an image build; image acquisition, Buildx execution, compilation, cache/output placement, and cleanup require the next separate operation.
- SBS-032: `BLD-ACORE-WOTLK-001` produces only the approved clean `authserver`, `worldserver`, `db-import`, and `tools` local images; a `client-data`, development-server, database-service, or MySQL image is not an approved output of this milestone.
- SBS-033: Client files and extracted data shall originate only from a separately governed protected client acquisition; the baseline build shall not construct or run the upstream client-data downloader target.
- SBS-034: Building extractor tooling does not authorize executing extractors. Extractor execution, protected-client mounts, output validation, and generated-data retention require a later bounded operation.
- SBS-035: Building the database-import image does not authorize database creation, connection, migration, import, or volume creation.
- SBS-036: Building authserver and worldserver images does not authorize starting either service, generating active configuration, publishing a port, or claiming runtime/gameplay compatibility.
- SBS-037: Baseline images remain local until registry identity, authentication, disclosure, retention, rollback, and publication governance is approved; no implicit registry push is permitted.
- SBS-038: Every baseline rebuild shall record the official base-image index and platform-manifest digests plus the acquisition-time package manifest. Mutable package repositories prevent a fully hermetic reproducibility claim unless later snapshot or artifact pinning is approved.
- SBS-039: Successful image construction advances build evidence only. Database, runtime, client, gameplay, and network milestones remain independently validated boundaries at lifecycle S0 unless separately advanced.
