# ADR-0011: Rootless Docker fuse-overlayfs Storage-Backend Remediation

- Status: Accepted at S0 — Proposed
- Date: 2026-07-14
- Scope: Storage-backend remediation for the existing rootless Docker 29.6.1 runtime
- Related evidence: `ADR-0010`, `RUN-0009`, `VAL-0009`

## Context

`RUN-0009` installed and contained Docker Engine 29.6.1, Docker Compose v5.3.1, and RootlessKit 3.0.1 successfully. The daemon runs as UID 1000, uses the rootless context and `unix:///run/user/1000/docker.sock`, keeps data at `/home/deck/.local/share/docker` on internal ext4, exposes no TCP API, and leaves SteamOS read-only mode and `Linger=no` unchanged.

The first container could not start. Engine 29's fresh-install containerd image store selected the containerd overlayfs snapshotter, and the kernel returned `EINVAL` with: `overlay: case-insensitive capable filesystem on /home/deck/.local/share/docker/.../work not supported`. The snapshotter did not fall back to `fuse-overlayfs`. `VAL-0009` therefore reported containment/integrity PASS and functional milestone FAIL.

## RUN-0009 failure evidence

- The three approved images pulled successfully and remain in the containerd-backed store.
- Container creation reached the overlayfs snapshotter but failed before workload execution.
- The observed failure concerns the storage backend, not Docker version, rootless identity, socket, context, data-root placement, network selection, or Compose installation.
- Pre-remediation inventory measured the preserved containerd backend at 19,357,601 bytes and 531 files, with aggregate content-list SHA-256 `33e457583f03ef7daf5cbb8e7f2a48a3d29229d0758c8a901c8177d0c669114f`.

## Official-source evidence

Official Docker documentation and official Moby source metadata were inspected read-only on 2026-07-14:

| Source | Relevant evidence |
|---|---|
| `https://docs.docker.com/engine/storage/containerd/` | Engine 29 fresh installs default to the containerd image store; its default snapshotter is overlayfs; switching stores hides prior images and containers while retaining their data. |
| `https://docs.docker.com/reference/cli/dockerd/` | The `containerd-snapshotter` feature can be disabled, `storage-driver` selects a classic driver, and `dockerd --validate` provides non-starting configuration validation. |
| `https://docs.docker.com/engine/storage/drivers/select-storage-driver/` | `fuse-overlayfs` is the preferred rootless compatibility driver when rootless `overlay2` cannot be used; `vfs` is a poor-performance diagnostic/testing choice. |
| `https://docs.docker.com/engine/daemon/` | Rootless daemon configuration belongs at `~/.config/docker/daemon.json`. |
| `https://docs.docker.com/engine/security/rootless/tips/` | Rootless daemon config, data, socket, and user-service locations match ADR-0010. |
| `https://github.com/moby/moby/blob/master/daemon/daemon.go` | Moby tracks whether the snapshotter feature is in use and determines the image-store choice during daemon startup. |

No Docker executable, archive, installer, repository, or image was acquired during this source check.

## Decision

Use Docker's classic image storage with the `fuse-overlayfs` storage driver for this rootless runtime. Install the following effective rootless daemon configuration at `/home/deck/.config/docker/daemon.json`:

```json
{
  "features": {
    "containerd-snapshotter": false
  },
  "storage-driver": "fuse-overlayfs"
}
```

This disables the containerd snapshotter image-store integration because its overlayfs snapshotter is incompatible with the observed case-insensitive-capable backing-filesystem behavior. `fuse-overlayfs` is the documented rootless compatibility backend.

Docker Engine remains 29.6.1, Docker Compose remains v5.3.1, RootlessKit remains unchanged at 3.0.1, and Docker remains rootless. The approved data root remains `/home/deck/.local/share/docker`; the approved socket remains `/run/user/1000/docker.sock`. RootlessKit's installer-selected network and port-driver configuration remains unchanged. `Linger=no` remains in force. No rootful daemon is introduced.

This ADR supersedes only the storage-backend portion of ADR-0010. All other ADR-0010 security, persistence, path, network, and lifecycle boundaries remain in force.

## Exact effective daemon configuration

Only `features.containerd-snapshotter=false` and `storage-driver=fuse-overlayfs` are required. No `hosts`, TCP endpoint, insecure-registry, data-root, networking, or firewall setting is added.

## Alternatives considered

| Alternative | Disposition |
|---|---|
| Keep the containerd overlayfs snapshotter | Rejected for this host baseline: it produced the reproduced kernel `EINVAL` before the first container started. |
| Classic native `overlay2` | Not selected: the failure identifies an overlayfs/backing-filesystem incompatibility, while Docker documents `fuse-overlayfs` as the rootless compatibility choice. |
| `vfs` | Rejected: it is a low-performance diagnostic fallback and is not approved for the AzerothCore runtime. |
| `btrfs` or `zfs` | Rejected: they do not match the approved internal ext4 data-root architecture. |
| Another Docker version or another data root | Not authorized and unnecessary for this narrow remediation. |

## Backend-switch consequences

The classic backend has a separate visible image/container inventory. Existing containerd-backed images and metadata are preserved but become hidden while the classic backend is active. The approved images may be pulled again into the classic backend for validation, creating temporary duplicate storage.

No automatic or manual data migration is authorized. Switching back would make the prior backend visible again, subject to later validation.

## Hidden prior-backend data

The RUN-0009 containerd backend under `/home/deck/.local/share/docker/containerd` must not be deleted, altered, or treated as migrated. Its paths, byte count, file count, hashes, and post-operation status must be recorded. Deletion requires a later governed cleanup decision.

## Rollback

If the new configuration prevents a safe rootless daemon start, preserve logs, remove only the newly created `daemon.json` (or restore an exact pre-existing backup), and restart the prior rootless configuration. Do not delete either backend. After a successful remediation, rollback is not automatic; a separately governed action would stop the user service, restore the exact prior configuration state, restart it, and verify rootless containment and prior-store visibility.

## Deferred cleanup

- Any deletion of the hidden containerd-backed images or metadata.
- Any backend migration.
- Buildx acquisition if the build-path probe shows it is absent.
- Lingering, LAN publication, firewall changes, AzerothCore acquisition, database creation, modules, addons, and client assets.

## Security boundaries

The daemon remains UID 1000 rootless, on the local user Unix socket only, with no TCP API, root daemon, privileged workload, host networking, sensitive host mounts, firewall change, SteamOS immutability change, lingering change, or external-drive runtime data. The storage switch must not force or change RootlessKit networking.

## Lifecycle statement

This decision remediates Docker storage only. It acquires no AzerothCore or client content and does not create an AzerothCore runtime. The project lifecycle remains **S0 — Proposed**.
