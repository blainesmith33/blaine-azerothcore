# Temporary Staging Workspace Requirements

Status: Authoritative  
Lifecycle: S0 — Proposed  
Decision authority: ADR-0005

- TSR-001: Governance remains authoritative under /run/media/deck/blAIneXPLAT/azerothcore/.
- TSR-002: Staging writes remain within /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/.
- TSR-003: Staging is temporary and not the permanent trusted runtime.
- TSR-004: No path may escape either authorized root.
- TSR-005: Only documentation, inactive templates, empty workspaces, scripts, manifests, and migration controls are allowed at S0.
- TSR-006: Acquired source, builds, live databases, container storage, client assets, services, sockets, runtime state, realms, and real secrets are prohibited.
- TSR-007: The staging drive must not hold the only copy of unique data.
- TSR-008: Paths are centralized; NOT_YET_SELECTED destinations are not created.
- TSR-009: Scripts reject missing, unresolved, traversing, and out-of-root paths.
- TSR-010: Future modifying utilities provide dry-run behavior.
- TSR-011: Created staging files receive and pass a SHA-256 manifest.
- TSR-012: Migration requires checksum-backed transfer and post-copy VAL evidence.
- TSR-013: Live database files are not copied as migration; approved export and import are required.
- TSR-014: Exact source, module, toolchain, configuration, database, and client revisions precede validated migration.
- TSR-015: AUD-0002 historical USB/UAS instability remains a known risk.
- TSR-016: Pre- and post-write host mount and bounded kernel-journal gates are required.
- TSR-017: New unresolved current-attachment I/O, USB/UAS, ext4, or journal failure prevents a full-success claim.

