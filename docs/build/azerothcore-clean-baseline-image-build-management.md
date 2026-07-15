# AzerothCore Clean Baseline Image Build Management

Status: Authoritative

Lifecycle: S0 — Proposed

Decision authority: ADR-0016

## Governed build

`BLD-ACORE-WOTLK-001` uses `CHK-ACORE-WOTLK-001`, detached at source pin `34a8bd6655c02448d3da6195dcd00647f634dde3`, as the sole source context. The Dockerfile is `apps/docker/Dockerfile` with SHA-256 `5a43e93dd29b10efdf09807517aa782ba7256ce1870039b1a2f94bbcf33c7b6e`.

The approved outputs are:

| Target | Local image tag | Purpose |
|---|---|---|
| `worldserver` | `blaine-azerothcore-worldserver:src-34a8bd6655c0-clean` | Compiled world server, not started |
| `authserver` | `blaine-azerothcore-authserver:src-34a8bd6655c0-clean` | Compiled authentication server, not started |
| `db-import` | `blaine-azerothcore-db-import:src-34a8bd6655c0-clean` | Import tooling and source SQL, not executed |
| `tools` | `blaine-azerothcore-tools:src-34a8bd6655c0-clean` | Client-data extractors, not executed |

The `client-data` and development-server targets are not approved outputs. Compose is not used for this milestone.

## Base image and build parameters

The base is official `docker.io/library/ubuntu:24.04`, pinned by OCI index digest `sha256:4fbb8e6a8395de5a7550b33509421a2bafbc0aab6c06ba2cef9ebffbc7092d90`. The Linux AMD64 child manifest is `sha256:52df9b1ee71626e0088f7d400d5c6b5f7bb916f8f0c82b474289a4ece6cf3faf`.

Builds use the existing `rootless` Docker-driver builder, `linux/amd64`, plain progress, `--pull`, `--load`, and disabled provenance/SBOM export. They do not push or use external cache exporters.

Common build arguments are:

```text
UBUNTU_VERSION=24.04@sha256:4fbb8e6a8395de5a7550b33509421a2bafbc0aab6c06ba2cef9ebffbc7092d90
TZ=Etc/UTC
USER_ID=1000
GROUP_ID=1000
DOCKER_USER=acore
CTOOLS_BUILD=all
CTYPE=RelWithDebInfo
CCACHE_CPP2=true
CSCRIPTPCH=OFF
CSCRIPTS=static
CMODULES=static
CSCRIPTS_DEFAULT_LINKAGE=static
CWITH_WARNINGS=ON
CMAKE_EXTRA_OPTIONS=
```

Every final image must carry the official source URL, exact source revision, `BLD-ACORE-WOTLK-001`, `SRC-ACORE-WOTLK-001`, `CHK-ACORE-WOTLK-001`, the blAIne project label, and `clean-unmodified` baseline label.

## Evidence layout

Non-secret evidence is retained internally at:

`/home/deck/.local/share/blAIne/azerothcore/build-evidence/BLD-ACORE-WOTLK-001/`

It contains separate build logs, base-image inspection, image inspect/history, package manifests, binary/ELF/dependency evidence, resource inventories, and `evidence-sha256.txt`. It must not contain source exports, image archives, credentials, client data, databases, or copied compiled binaries.

## Required validation

Before build, validate the exact detached clean checkout, source tree and archive digest, Dockerfile identity, absence of custom modules, rootless Docker, Buildx version and builder, `fuse-overlayfs`, resource thresholds, empty target tags, and evidence-path absence.

After build, validate:

- four Linux AMD64 local images and their governed labels;
- non-root image user and expected target command;
- expected server/import/extractor paths, modes, SHA-256 values, ELF architecture, and shared-library resolution without executing them;
- sorted Ubuntu package manifests;
- configuration templates without generating active configuration;
- empty client-data output paths;
- no client-data/MySQL/dev image, database, container, volume, operation network, process, listener, or push;
- unchanged Docker context/configuration and clean source checkout; and
- a complete evidence checksum manifest and content scan.

Shell-only inspection containers use network none, a read-only root, dropped capabilities, no-new-privileges, an overridden shell entrypoint, no explicit mounts, and automatic removal. Server, importer, and extractor executables are never invoked.

## Cache and package mutability

The existing rootless builder's internal cache may remain because it is expected build-system state and materially supports later review/rebuild. Do not use `docker system prune`, broad builder pruning, or unfiltered Buildx pruning.

The source and Ubuntu image manifests are pinned. Ubuntu package repositories are not snapshot-pinned, so package versions are acquisition-time inputs. Capture full package manifests on every rebuild and treat differing packages as a reproducibility-relevant change.

## Rebuild, failure recovery, and removal

A rebuild requires a new bounded operation and must not overwrite an existing governed tag without explicit comparison/removal authorization. On failure, stop subsequent targets, retain logs and cache, preserve any successful images, and record the partial state. Do not automatically prune or roll back the runtime.

Removal requires exact image-ID/label verification and dependent-container checks. Remove only named governed images under separate authority. Cache removal, source removal, and runtime removal are separate operations.

## Deferred operations

Client acquisition is a separate artifact-governance boundary. Extractors may be run only against a later approved protected client copy. Building `db-import` does not authorize a database. Building server images does not authorize server startup. Ports and ingress remain governed by the network-boundary architecture and later deployment decisions.

No AzerothCore service has been started by this build record. Lifecycle remains **S0 — Proposed**.
