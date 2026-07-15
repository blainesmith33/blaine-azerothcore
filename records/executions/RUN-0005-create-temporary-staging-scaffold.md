# RUN-0005 — Create Temporary Staging Scaffold

## Record control

- Identifier: RUN-0005
- Start: 2026-07-14T17:41:14-07:00
- Completion: 2026-07-14T18:03:47-07:00
- Working directory: /home/deck
- Lifecycle: S0 — Proposed
- Result: scaffold created and validated
- Related authority: ADR-0005 and ADR-0006
- Related evidence: INC-0001, AUD-0002, RUN-0004, VAL-0004, VAL-0005

## Host-write authorization boundary

Writes were limited to:

1. /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/
2. The explicitly authorized governance documents and RUN-0005/VAL-0005 under /run/media/deck/blAIneXPLAT/azerothcore/

No other path was modified.

## Resolved roots

- Governance: /run/media/deck/blAIneXPLAT/azerothcore
- Staging parent: /run/media/deck/blAIne-476GB-SAM/blAIne
- Staging: /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging

## Pre-write gate

PASS:

- RUN-0001 through RUN-0004 and VAL-0001 through VAL-0004 existed.
- INC-0001 and AUD-0002 existed.
- The staging parent existed and the staging root did not.
- ADR-0005, ADR-0006, RUN-0005, VAL-0005, and the other new governance paths did not exist.
- Host findmnt identified /dev/sdb1, ext4, rw,nosuid,nodev,noatime,errors=remount-ro.
- Disk and partition block read-only flags were zero.
- The current attachment mounted read/write at 2026-07-14T17:25:57-07:00.
- The bounded kernel journal through 17:41:27 contained zero new relevant errors.

## Device and filesystem observations

- Disk: /dev/sdb, Samsung SSD 830 Series, 512110190592 bytes, USB, RO=0.
- Partition: /dev/sdb1, 512108789760 bytes, ext4, label blAIne-476GB-SAM, UUID 70913570-fb08-4b22-a534-a8e658cb86d4, RO=0.
- Mountpoint: /run/media/deck/blAIne-476GB-SAM.
- Host options: rw,nosuid,nodev,noatime,errors=remount-ro.
- Available space at gate: 477178753024 bytes.
- Staging parent: deck:deck, mode 0755.
- Staging root after creation: deck:deck, mode 0755.
- Historical USB/UAS instability from AUD-0002 remains a warning.

Device serial and path identifiers require sanitization before public release.

## Minimal write probe

Commands created the exact staging root, wrote fixed value AZEROTHCORE_STAGING_WRITE_PROBE_V1 to .staging-write-probe, compared it, renamed it to .staging-write-probe-renamed, compared it again, removed it, and asserted that neither file remained.

Results:

- Initial comparison: PASS.
- Renamed comparison: PASS.
- Cleanup: PASS.
- Exit: 0.
- Immediate host mount: read/write.
- New relevant post-probe kernel lines: 0.

## Governance files created

- docs/architecture/temporary-staging-and-migration-architecture.md
- docs/architecture/server-progression-and-promotion-architecture.md
- docs/requirements/temporary-staging-workspace-requirements.md
- docs/requirements/server-build-sequence-requirements.md
- records/decisions/ADR-0005-temporary-portable-staging-workspace.md
- records/decisions/ADR-0006-baseline-first-sequential-research-and-integration.md
- records/executions/RUN-0005-create-temporary-staging-scaffold.md
- records/validations/VAL-0005-temporary-staging-scaffold-validation.md

## Governance files narrowly amended

- README.md
- GOVERNANCE.md
- docs/governance/project-charter.md
- docs/requirements/acceptance-criteria.md
- docs/instances/instance-classification.md

## Directories created

    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/backups
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/environment
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/exports
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/backups
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/compose
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/config
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/database
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/logs
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/modules
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/records
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/backups
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/compose
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/config
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/database
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/logs
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/modules
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/records
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/backups
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/compose
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/config
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/database
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/logs
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/modules
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/records
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/backups
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/compose
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/config
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/database
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/logs
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/modules
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/records
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/backups
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/compose
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/config
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/database
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/logs
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/modules
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/records
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/logs
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/manifests
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/migration
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/modules
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/modules/blaine
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/modules/candidates
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/modules/manifests
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/modules/patches
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/modules/upstream-reference
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/records
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/records/builds
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/records/instances
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/records/migrations
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/records/validations
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/scripts
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/client-data
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/compose
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/config
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/source
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/templates
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/tooling

## Files created under staging

    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/.gitignore
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/STAGING-MANIFEST.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/backups/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/environment/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/environment/common.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/environment/paths.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/environment/secrets.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/exports/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/INSTANCE-MANIFEST.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/backups/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/compose/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/config/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/database/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/instance.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/logs/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/modules/module-manifest.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/baseline/records/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/INSTANCE-MANIFEST.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/backups/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/compose/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/config/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/database/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/instance.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/logs/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/modules/module-manifest.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/development/records/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/INSTANCE-MANIFEST.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/backups/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/compose/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/config/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/database/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/instance.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/logs/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/modules/module-manifest.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/disposable/records/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/INSTANCE-MANIFEST.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/backups/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/compose/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/config/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/database/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/instance.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/logs/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/modules/module-manifest.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/integrated/records/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/INSTANCE-MANIFEST.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/backups/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/compose/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/config/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/database/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/instance.env.example
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/logs/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/modules/module-manifest.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/instances/research/records/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/logs/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/manifests/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/manifests/instance-registry.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/manifests/module-registry.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/manifests/source-lock-template.yaml
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/manifests/staging-paths-manifest.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/migration/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/migration/checksum-manifest-template.txt
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/migration/migration-checklist.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/migration/migration-plan-template.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/migration/permanent-target-requirements.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/modules/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/records/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/records/validations/VAL-0005-created-files.sha256
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/scripts/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/scripts/create-instance-scaffold.sh
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/scripts/migration-preflight.sh
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/scripts/show-environment-paths.sh
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/scripts/validate-staging-layout.sh
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/client-data/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/compose/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/config/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/source/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/templates/README.md
    /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/shared/tooling/README.md

The checksum file is intentionally not listed inside its own 84-entry payload.

## Command and exit-status ledger

- Authority/history/non-overwrite/mount/device gate: exit 0.
- Bounded current-attachment journal gate: exit 0; zero matching error lines.
- Read-only inspection of five existing authoritative documents: exit 0.
- mkdir of exact staging root: exit 0.
- Fixed write/read/rename/read/delete probe: exit 0.
- Immediate post-probe mount/journal gate: exit 0.
- Explicit staging directory creation: exit 0.
- Governed staging-file patches: exit 0.
- First generated instance-file patch: failed before writes because its patch hunk was malformed.
- Corrected generated instance-file patch: exit 0.
- Intermediate host mount/journal gate after slow metadata writes: exit 0.
- First combined governance patch: failed before writes because an expected context line did not match.
- Governance document creation patch: exit 0.
- Narrow amendment patch: exit 0.
- Heading-order correction patch: exit 0.
- chmod 0755 on the four staging scripts: exit 0.
- bash -n on all four scripts: exit 0 each.
- First layout-validator execution: exit 1 because the secrets template had an unintended blank line.
- Read-only bash -x diagnosis: exit 1 at the exact five-line assertion.
- Narrow removal of the blank line: exit 0.
- Corrected layout-validator execution: exit 0.
- SHA-256 manifest generation: exit 0; 84 payload entries.
- sha256sum -c before sync: exit 0.
- sync -f against the staging root: exit 0.
- sha256sum -c after sync: exit 0.
- Post-write host mount/journal gate: exit 0; zero matching error lines.
- First comprehensive content validation: exit 1 on a case-sensitive phrase mismatch in the validation command.
- Corrected comprehensive content validation: exit 0.
- RUN-0005/VAL-0005 record creation: exit 0.
- Final validation and printing: recorded after creation; exit 0.
- Complete-file printing exposed literal plus arguments in four script help functions; the first correction patch missed exact escaped context and made no change.
- Exact-line help correction: exit 0.
- bash -n and --help execution for all four corrected scripts: exit 0 each.
- SHA-256 manifest regeneration, verification, scoped sync, and second verification after script correction: exit 0.
- Final post-correction mount and bounded journal gate: exit 0; zero matching error lines.

## Script results

All scripts begin with a Bash shebang followed by set -euo pipefail, implement help, reject missing/unresolved paths, and contain no privilege, package, container-runtime, network-download, or database execution command.

- validate-staging-layout.sh: bash -n PASS; read-only layout run PASS.
- show-environment-paths.sh: bash -n PASS; not invoked against unresolved template.
- create-instance-scaffold.sh: bash -n PASS; not invoked.
- migration-preflight.sh: bash -n PASS; not invoked against a permanent target.
- Executable modes were preserved as 0755 on ext4.

## Integrity and post-write evidence

- Checksum manifest: records/validations/VAL-0005-created-files.sha256.
- Payload entries: 84.
- Verification before sync: PASS, exit 0.
- Filesystem-scoped sync: PASS, exit 0.
- Verification after sync: PASS, exit 0.
- Final host mount: /dev/sdb1 ext4 read/write.
- Bounded post-write journal interval began at 2026-07-14T17:41:27-07:00.
- New matching EXT4, remount, I/O, Buffer I/O, offline, USB reset, UAS error, or journal-abort lines: 0.

## Warnings and unexpected behavior

- Historical USB/UAS instability remains unresolved and staging must not hold the only copy of unique data.
- Small-file metadata patches were unusually slow, but the host mount remained read/write and no new relevant kernel errors were recorded.
- Two authoring patches failed validation before writing and were corrected.
- The first layout validation exposed and corrected one blank template line.
- The first comprehensive validation used incorrect case in its search phrase; the exact field was present and the corrected check passed.
- Complete printing exposed a help-output formatting defect. All four in-scope scripts were corrected, their help output was executed successfully, and the checksum and post-write gates were rerun.
- No failed or unexpected behavior was silently omitted.

## Non-modification attestation

No package, Docker, Podman, source, module, client, container, image, database, service, firewall, network, mount, unmount, remount, filesystem repair, compile, repository, Git, credential, live-runtime, or permanent-runtime operation occurred.

The project remains S0 — Proposed.
