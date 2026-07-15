# Docker Buildx User-Plugin Management

- Status: Authoritative tooling policy at S0
- Lifecycle: S0 — Proposed
- Decision: ADR-0013
- Validation: VAL-0013

## Governed installation

| Property | Required value |
|---|---|
| Buildx version | `v0.35.0` |
| Plugin path | `/home/deck/.docker/cli-plugins/docker-buildx` |
| SHA-256 | `d41ece72044243b4f58b343441ae37446d9c29a7d6b5e11c61847bbcf8f7dfda` |
| Owner | `deck:deck` |
| Mode | `0755` |
| Storage | Internal ext4 storage beneath `/home/deck` |
| Official source class | Stable release asset and checksum manifest from the official `docker/buildx` repository |

The plugin is user-scoped application tooling. It must not be installed into the SteamOS image, a system plugin directory, the portable project drive, or the Git repository.

## Verification procedure

Before an installation or update:

1. Confirm the active user, host operating system, and architecture.
2. Verify official tag metadata: exact tag, not draft, not prerelease, official repository identity, exact Linux AMD64 asset, and official checksum manifest.
3. Require exactly one applicable manifest entry.
4. Download the asset only to owner-only internal temporary storage.
5. Compare its SHA-256 before execution or installation.
6. Confirm a regular Linux x86-64 executable and invoke it directly to verify the embedded version.
7. Install atomically at the governed path with owner `deck:deck` and mode `0755`.
8. Recheck the installed checksum, direct version, and `docker buildx version`.
9. Verify Docker context, storage driver, daemon configuration hashes, builder inventory, native scratch build behavior, cleanup, and residual cache.

Current verification commands include:

```bash
stat /home/deck/.docker/cli-plugins/docker-buildx
sha256sum /home/deck/.docker/cli-plugins/docker-buildx
file /home/deck/.docker/cli-plugins/docker-buildx
/home/deck/.docker/cli-plugins/docker-buildx version
docker buildx version
```

## Builder selection

The active Docker context is `rootless`, and its automatically associated Docker-driver builder is also named `rootless`. Direct builds use:

```bash
docker buildx build --builder rootless ...
```

The builder literally named `default` belongs to the unused default Docker context. It resolves to an unavailable rootful endpoint because no rootful Docker daemon exists. That condition is expected and must not be repaired by starting a rootful daemon or creating another builder.

The current Compose/Buildx combination rejects explicit references to the context-derived builder even while `rootless` is selected. Validated Compose builds therefore use the already-selected `rootless` context builder implicitly:

```bash
docker compose --project-name <project> --file <compose-file> build --progress plain
```

Before and after such a build, evidence must show `docker context show` = `rootless`, `docker buildx ls` marks `rootless` selected and running, no manual builder instance exists, and the resulting image is loaded into the rootless daemon. `BUILDX_BUILDER=rootless` remains the preferred explicit policy when a compatible Compose/Buildx combination honors it; current incompatibility must not be worked around by changing context or creating a builder.

## Build scope and prohibitions

Current approval is native Linux AMD64 only through the existing Docker-driver builder. It does not authorize:

- QEMU or `binfmt_misc` registration;
- multi-platform emulation;
- privileged helpers or privileged containers;
- a Docker-container, Kubernetes, cloud, remote, multi-node, or additional builder;
- registry authentication or image pushes;
- host networking, port publication, or network-policy changes; or
- Docker daemon, context, storage-driver, RootlessKit, firewall, or SteamOS changes.

## Update procedure

Buildx has been installed manually and will not update through a system package manager. Every update requires a separately governed operation that:

1. records the operational or compatibility reason;
2. verifies the release is official and appropriate;
3. pins the exact version and Linux AMD64 asset;
4. records official and actual checksums;
5. inventories and, if authorized, backs up the existing plugin;
6. validates Docker Engine and Compose compatibility;
7. installs atomically without changing Docker configuration;
8. repeats direct and Compose native scratch builds;
9. removes disposable resources without broad pruning; and
10. records rollback and publication evidence.

No moving `latest` reference may serve as the final pin.

## Rollback and removal

For an update failure, preserve diagnostic evidence and restore only the exact prior verified plugin from an operation-authorized backup. Do not automatically change builders or Docker configuration.

Separately authorized plugin removal deletes only:

```text
/home/deck/.docker/cli-plugins/docker-buildx
```

The legitimate `/home/deck/.docker/buildx/` client-state directory is not the plugin. It may contain locks, activity, selected-context metadata, and builder metadata and must not be removed without a separate inventory. BuildKit cache must not be broadly pruned as part of plugin removal.

## Evidence required before future AzerothCore builds

The Buildx prerequisite is established only for native scratch builds. Before AzerothCore is built, a later operation must acquire and pin the official source, inspect its build definitions and Compose requirements, confirm compatibility with Docker Engine 29.6.1, Compose v5.3.1, Buildx v0.35.0, and the `rootless` Docker-driver builder, and define bounded build and cleanup evidence.

No AzerothCore source was acquired or built by this tooling validation. The lifecycle remains **S0 — Proposed**.
