# ADR-0013: User-Scoped Docker Buildx Plugin and Rootless Context Builder

- Status: Accepted at S0 — Proposed
- Date: 2026-07-14
- Scope: Native-platform Docker build tooling for the existing rootless runtime
- Related decisions: `ADR-0010`, `ADR-0011`
- Related evidence: `RUN-0013`, `VAL-0013`

## Context

ADR-0010 selected a user-scoped rootless Docker Engine, and ADR-0011 selected classic `fuse-overlayfs` image storage. Docker Engine 29.6.1 and Docker Compose v5.3.1 are operational through the `rootless` Docker context. Building the future clean AzerothCore baseline requires the Docker Buildx CLI plugin, but the SteamOS image must remain read-only and no system package or privileged builder is approved.

The initial RUN-0013 attempt installed and verified the official Buildx v0.35.0 plugin, then stopped before building because it incorrectly required a builder literally named `default`. On this host, context-derived Docker-driver builders use their Docker context names. The usable builder is therefore `rootless`. The entry named `default` belongs to the unused default Docker context, resolves to the absent rootful socket, and is not a prerequisite for this rootless runtime.

## Decision

Use the checksum-verified official Docker Buildx v0.35.0 Linux AMD64 binary as a user-scoped Docker CLI plugin under `/home/deck/.docker/cli-plugins/docker-buildx`, and use the Docker-driver builder automatically associated with the active `rootless` Docker context for native-platform builds.

The pinned artifact and installation properties are:

| Property | Approved value |
|---|---|
| Version | `v0.35.0` |
| Official source | `docker/buildx` GitHub release assets for tag `v0.35.0` |
| Asset | `buildx-v0.35.0.linux-amd64` |
| SHA-256 | `d41ece72044243b4f58b343441ae37446d9c29a7d6b5e11c61847bbcf8f7dfda` |
| Plugin path | `/home/deck/.docker/cli-plugins/docker-buildx` |
| Owner and mode | `deck:deck`, `0755` |
| Storage | Internal ext4 storage under `/home/deck` |
| Builder | Context-derived `rootless` builder, Docker driver |
| Current platform scope | Native Linux AMD64 only |

There is no requirement for a builder literally named `default`. No extra builder may be created, renamed, removed, or selected without later authorization. The unavailable rootful `default` entry is an expected consequence of retaining an unused default context without a rootful daemon.

## Official verification requirement

Every installation or update must verify official release metadata, reject drafts and prereleases unless a later ADR explicitly approves one, select the exact platform asset, obtain the official checksum manifest, require exactly one matching checksum entry, compare the downloaded artifact before installation, verify executable type and embedded version, and recheck the installed file afterward.

The binary and checksum manifest are installation inputs, not repository artifacts. Neither belongs in Git.

## Builder policy

The existing `rootless` Docker-driver builder is approved for current native Linux AMD64 development builds. Direct Buildx commands should name it explicitly where supported. Docker Compose may use the already-selected context-derived builder implicitly when explicit context-builder selection is rejected by the current Compose/Buildx integration; the active Docker context, selected Buildx builder, build output, and post-build invariants must prove that the rootless builder was used.

The following remain prohibited without a later decision:

- `docker buildx create`, additional nodes, or separate Docker-container, Kubernetes, cloud, or remote builders;
- QEMU, `binfmt_misc`, privileged helpers, or multi-platform emulation;
- registry authentication or image pushes;
- privileged build containers or host-sensitive mounts; and
- changes to the Docker context, daemon configuration, storage driver, rootless networking, or system package state.

## Manual update responsibility

This user-scoped binary has no package-manager update channel. Buildx updates are manual governed operations. A later update must document the reason, compatibility with the installed Docker Engine and Compose versions, stable release status, exact asset, checksum, direct and CLI-reported versions, builder discovery, native scratch builds, cleanup, rollback, and Git publication evidence. No update may silently follow a moving release.

Update automation is deferred.

## Rollback and removal

Rollback of a failed future update must stop before building, preserve evidence, and atomically restore the exact previously verified plugin if a backup was explicitly created for that update. Removal, when separately authorized, deletes only `/home/deck/.docker/cli-plugins/docker-buildx` after confirming it is the governed artifact.

The legitimate client-state directory `/home/deck/.docker/buildx/` is separate from the plugin binary. It may contain locks, selected-context metadata, activity, and builder metadata. It must not be deleted as part of routine plugin rollback or removal unless a separate inventory and cleanup authorization proves every entry is operation-owned. BuildKit cache is also separate and must not be broadly pruned.

## Relationship to AzerothCore build work

This decision establishes the build-tool prerequisite only. A later governed operation must acquire and pin the official AzerothCore source before any clean baseline build. Buildx validation does not validate AzerothCore source, its Compose model, compilation, databases, runtime services, or clients.

## Explicit non-authorization and lifecycle

This ADR does not authorize AzerothCore, ChromieCraft, module, client, or documentation-corpus acquisition; an AzerothCore build; multi-platform support; registry publishing; a new builder; a rootful daemon; or any network exposure. The lifecycle remains **S0 — Proposed**.
