# ADR-0016: Clean Unmodified AzerothCore Baseline Image Build

- Status: Accepted at S0 — Proposed
- Date: 2026-07-15
- Scope: Native Linux AMD64 server and extraction-tool image construction
- Related decisions: `ADR-0013`, `ADR-0014`, `ADR-0015`
- Related evidence: `BLD-ACORE-WOTLK-001`, `RUN-0016`, `VAL-0016`

## Context

ADR-0014 pinned official source identity as `SRC-ACORE-WOTLK-001`. ADR-0015 established `CHK-ACORE-WOTLK-001` as a faithful, independent internal-ext4 checkout at commit `34a8bd6655c02448d3da6195dcd00647f634dde3` and tree `25ad25f5fb8e119f68fef69b080934f1182ad8d6`. ADR-0013 established Docker Buildx v0.35.0 and the Docker-driver builder associated with the active rootless context.

The pinned `apps/docker/Dockerfile` has separate `authserver`, `worldserver`, `db-import`, `client-data`, and `tools` final targets. Its shared build stage compiles all server applications and tools. The `client-data` target is a distinct downloader and is outside this milestone.

## Decision

Build the clean pinned AzerothCore baseline from `CHK-ACORE-WOTLK-001` using the existing rootless Buildx Docker-driver builder, a recorded official Ubuntu 24.04 digest, and direct Dockerfile target builds for `authserver`, `worldserver`, `db-import`, and `tools`; preserve those four local images and internal BuildKit cache; and exclude client-data acquisition, database execution, service startup, Compose orchestration, and port publication.

| Property | Decision |
|---|---|
| Build identifier | `BLD-ACORE-WOTLK-001` |
| Source / checkout | `SRC-ACORE-WOTLK-001` / `CHK-ACORE-WOTLK-001` |
| Source pin / tree | `34a8bd6655c02448d3da6195dcd00647f634dde3` / `25ad25f5fb8e119f68fef69b080934f1182ad8d6` |
| Dockerfile | `apps/docker/Dockerfile`, SHA-256 `5a43e93dd29b10efdf09807517aa782ba7256ce1870039b1a2f94bbcf33c7b6e` |
| Builder | Existing context-derived `rootless` builder, Docker driver |
| Platform | Native `linux/amd64` only |
| Base tag | Official `docker.io/library/ubuntu:24.04` |
| Base index digest | `sha256:4fbb8e6a8395de5a7550b33509421a2bafbc0aab6c06ba2cef9ebffbc7092d90` |
| AMD64 child manifest | `sha256:52df9b1ee71626e0088f7d400d5c6b5f7bb916f8f0c82b474289a4ece6cf3faf` |
| Output | Load four local images into the existing rootless image store; no push |
| Evidence | `/home/deck/.local/share/blAIne/azerothcore/build-evidence/BLD-ACORE-WOTLK-001/` |

## Direct Buildx boundary

Direct Buildx target builds make the four authorized outputs explicit and avoid Compose interpolation, database-service definitions, client-data services, startup behavior, networks, volumes, and published ports. Compose remains installed but is not used by this build milestone.

The `db-import` image may contain the compiled importer and source-controlled SQL, but building it does not authorize running the importer, connecting to a database, creating a database volume, or applying migrations. The `tools` image exists solely to preserve compiled extractors for a later separately governed client-data operation. No extractor is executed here.

## Image and cache policy

The four governed tags are local-only:

- `blaine-azerothcore-authserver:src-34a8bd6655c0-clean`
- `blaine-azerothcore-worldserver:src-34a8bd6655c0-clean`
- `blaine-azerothcore-db-import:src-34a8bd6655c0-clean`
- `blaine-azerothcore-tools:src-34a8bd6655c0-clean`

Each image carries the exact source revision, source identifier, checkout identifier, build identifier, project identifier, and `clean-unmodified` classification. The successful images and the existing builder's internal cache are retained. No broad pruning is approved. Registry publication requires later registry, authentication, retention, and disclosure governance.

## Reproducibility classification

Source identity, source tree, Dockerfile, build arguments, Ubuntu image index, and Linux AMD64 child manifest are pinned and recorded. Ubuntu packages are the versions served by official Ubuntu repositories during this acquisition window and are captured in package manifests.

The build is therefore **identity-pinned but not fully hermetic**. The Dockerfile performs `apt-get update` and package installation against mutable Ubuntu repositories without a snapshot repository or per-package artifact digest pin. A later rebuild may receive different package versions even with the same source and base-image digest. Any rebuild must record the newly observed package set and must not claim bit-for-bit reproducibility without additional controls.

## Security and containment

- Rootless Docker, the `rootless` context, `fuse-overlayfs`, and the existing builder remain unchanged.
- No privileged helper, QEMU, `binfmt_misc`, host network, registry authentication, image push, Compose orchestration, port publication, or service process is allowed.
- Build-time network access is limited to the official Ubuntu base on Docker Hub and official Ubuntu package repositories.
- Client-data, ChromieCraft, WoW client, module, MySQL image, database, and runtime-service acquisition or execution remain prohibited.
- The checkout remains detached, clean, and unmodified before and after build.

## Update and rebuild

A rebuild must revalidate governance synchronization, checkout pin/tree/digest and cleanliness, Dockerfile identity, builder/runtime invariants, free resources, tag collisions, official base manifests, acquisition endpoints, final images, binaries, packages, containment, and evidence integrity. A source, Dockerfile, Buildx, Docker, base-image, package-policy, module, platform, or tag change requires an explicitly reviewed replacement or superseding record.

## Rollback and removal

Rollback is not executed by this decision. A later authorized removal may delete only the four exact governed tags after verifying IDs, labels, absence of dependent containers, and preservation of manifests and evidence. BuildKit cache removal is a separate decision and must not use broad pruning. The source checkout, source-object repository, rootless runtime, and governance evidence are not part of image rollback.

## Deferred boundaries and lifecycle

ChromieCraft or other client acquisition, extractor execution, client-data generation, database initialization/import, configuration activation, authserver/worldserver startup, Compose orchestration, gameplay validation, client compatibility, ports, and ingress remain later governed operations. Lifecycle remains **S0 — Proposed**.
