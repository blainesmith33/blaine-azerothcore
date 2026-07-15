# RUN-0013 — Install and Validate Docker Buildx User Plugin

## Record control

- Date: 2026-07-14 (America/Phoenix)
- Initial installation attempt: 2026-07-14T21:37:09-07:00 through approximately 21:40
- Corrective validation start: 2026-07-14T21:45:26-07:00
- Lifecycle before and after: `S0 — Proposed`
- Objective: install and verify Docker Buildx v0.35.0 as a user-scoped plugin, validate native scratch builds through the existing rootless context builder and Docker Compose, publish governance evidence, and stop before AzerothCore acquisition.
- Authoritative repository: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Branch: `main`
- Starting commit: `311be3d0e5178c8560f7a23da61c0806bea0c59c`
- Publication state in this revision: runtime validation and cleanup passed; commit and push verification pending.

## Authorization boundary

Persistent writes were limited to the authoritative repository, `/home/deck/.docker/cli-plugins/`, legitimate `/home/deck/.docker/buildx/` client state, and owner-only temporary files beneath `/run/user/1000`. The operation did not authorize or perform sudo, package installation, SteamOS modification, Docker daemon or context changes, builder creation/removal, QEMU or `binfmt_misc`, privileged helpers, registry authentication or push, port publication, network-policy changes, broad pruning, or AzerothCore/client/module/corpus acquisition.

## Repository starting state

- Canonical root: `/run/media/deck/blAIneXPLAT/azerothcore`.
- Filesystem: `/dev/sda1`, exFAT, mounted read/write.
- Branch `main`; clean working tree; no active merge, rebase, cherry-pick, revert, bisect, or sequencer operation.
- Origin matched the authorized SSH remote.
- Local HEAD, local `origin/main`, and remote `refs/heads/main` all equaled `311be3d0e5178c8560f7a23da61c0806bea0c59c`.
- The expected starting commit was current and therefore an ancestor.
- Tracked-file count: 82.
- README and GOVERNANCE reported `S0 — Proposed`.

## Initial installation attempt

The first attempt completed the following successfully:

- verified user `deck`, UID 1000, Linux x86-64, 64-bit;
- verified Docker client/server 29.6.1, Compose v5.3.1, rootless security, the `rootless` context and approved user socket, internal ext4 data root, `fuse-overlayfs`, and active/enabled user service;
- confirmed Buildx and its target file were initially absent;
- retrieved official Buildx v0.35.0 metadata, confirmed tag `v0.35.0`, draft=false, prerelease=false, publication time `2026-06-17T23:18:28Z`, official repository identity, and the exact expected asset;
- downloaded only `buildx-v0.35.0.linux-amd64` and `checksums.txt` to owner-only runtime storage;
- found exactly one applicable checksum entry and matched both the manifest and GitHub release digest;
- verified a regular static Linux x86-64 executable reporting `github.com/docker/buildx v0.35.0 a319e5b15052cf6557ceb666eb8ff6e32380b782`;
- installed the plugin atomically at the approved path and verified Docker CLI discovery; and
- preserved Docker daemon/configuration hashes, context, storage driver, socket, and rootless service state.

The first metadata parser incorrectly hardcoded a GitHub numeric release database ID. That local assertion failed even though the official repository paths and release properties were correct. It was corrected before asset download to validate the stable repository path rather than an unstable numeric ID.

The attempt then stopped before any build because the original prompt incorrectly required `docker buildx inspect default` to succeed. The `default` entry belongs to the unused default Docker context and resolves to the absent rootful socket. The active context-derived builder was already `rootless`. No rootful daemon was created and no builder was created, renamed, removed, or selected.

No image, container, network, volume, BuildKit build cache, repository file, commit, or push was created by the initial attempt. The verified plugin remained installed. Its temporary download directory was removed. The small client-state directory created by the first `docker buildx ls` was also removed because that attempt's authorization had not included it. No false PASS was assigned.

## Installed plugin evidence

| Property | Result |
|---|---|
| Installed path | `/home/deck/.docker/cli-plugins/docker-buildx` |
| Owner and mode | `deck:deck`, `0755` |
| Size and type | 65,265,826 bytes; regular static ELF 64-bit x86-64 executable |
| Asset | `buildx-v0.35.0.linux-amd64` |
| Expected SHA-256 | `d41ece72044243b4f58b343441ae37446d9c29a7d6b5e11c61847bbcf8f7dfda` |
| Installed SHA-256 | `d41ece72044243b4f58b343441ae37446d9c29a7d6b5e11c61847bbcf8f7dfda` |
| Direct version | `github.com/docker/buildx v0.35.0 a319e5b15052cf6557ceb666eb8ff6e32380b782` |
| Docker CLI plugin version | Same; exit 0 |
| Backing storage | `/dev/nvme0n1p8`, internal ext4 under `/home` |

The corrective operation did not redownload or overwrite the binary.

## Corrective runtime and provenance preflight

- Docker Engine remained 29.6.1; Compose remained v5.3.1; RootlessKit remained 3.0.1.
- Active context remained `rootless`, endpoint `unix:///run/user/1000/docker.sock`.
- Security options included rootless; Docker root remained `/home/deck/.local/share/docker`; storage driver remained `fuse-overlayfs`; driver status remained empty, consistent with disabled containerd snapshotter image storage.
- The user Docker service remained active and enabled. Root system Docker service/socket were inactive and `/var/run/docker.sock` was absent.
- `docker buildx ls` showed selected `rootless*`, Docker driver, status running, BuildKit v0.31.1, native `linux/amd64` capability. The `default` entry was an error against the absent rootful socket and was classified as unused, not as an installation failure.
- `docker buildx inspect rootless` exited 0. `/home/deck/.docker/buildx/instances` contained zero manual instance files.
- Official metadata and `checksums.txt` were retrieved again without downloading the binary. Tag, stable status, repository, exact asset, one applicable checksum, and installed hash all passed.

## Direct Buildx scratch validation

The direct context contained only a Dockerfile without a syntax directive and a 46-byte marker. It used `FROM scratch`, the two required labels, and `COPY marker.txt /marker.txt`.

Command:

```text
docker buildx build --builder rootless --load --progress=plain --tag blaine-azerothcore-buildx-validation:run-0013 <context>
```

Result: **PASS**, exit 0. Build output identified the `rootless` instance and Docker driver. It loaded only the Dockerfile, empty `.dockerignore`, local context, copy step, and image export; no base image or external Dockerfile frontend was resolved or pulled.

- Image ID: `sha256:3ba082331c0429a2b2375db24d8c37994a2d8816e5f9c132e23e74e03a9bc699`.
- Architecture/OS: `amd64` / `linux`.
- Labels: project `blaine-azerothcore`; validation `RUN-0013`.
- Exposed ports: absent.
- History: local copy and labels only, BuildKit Dockerfile frontend.
- A container named `blaine-buildx-run0013-direct-marker` was created but never started: state `created`, running=false, privileged=false, empty port bindings.
- `docker cp` extracted `/marker.txt` from the stopped container.
- Source and extracted marker SHA-256 were both `177ab73bfa595065b192b4cce324c339ae18776481c354cf3266694fb00eda65`; byte comparison exited 0.

One initial image-inspection template returned exit 1 because a scratch image omitted the `ExposedPorts` map key. A corrected JSON parse passed; this did not affect the build or container state.

The stopped container, image, copied marker, and direct context were removed before the Compose test.

## Docker Compose scratch validation

The separate Compose context contained the same scratch Dockerfile/marker and one service with a local build context, local image name, and validation labels. It defined no ports, volumes, secrets, credentials, host networking, or startup requirement. `docker compose config` exited 0.

Three explicit-selection forms stopped before building with the same diagnostic requesting the `rootless` context identity:

1. `BUILDX_BUILDER=rootless docker compose ... build` — exit 1.
2. `docker compose ... build --builder rootless` — exit 1.
3. Explicit same-context environment/global-context variants with `--builder rootless` — exit 1.

Each failed attempt produced no image, container, network, volume, or builder. The stored Docker context remained `rootless`. Because `docker buildx ls` already marked `rootless*` selected, the successful correction allowed Compose to use that selected context-derived builder implicitly without changing selection:

```text
docker compose --project-name blaine-buildx-run-0013 --file <compose-file> build --progress plain
```

Result: **PASS**, exit 0. Compose used BuildKit/Bake, loaded only local definitions and context, reused the validated local scratch copy layer, exported the local image, and performed no pull or push.

- Image ID: `sha256:d587021f351e3959c52fef15a339cf458470c0a4e2da2037a76581a0ed127e3b`.
- Architecture/OS and labels matched the direct build.
- Exposed ports: absent.
- Running and stopped project containers: zero.
- Project networks and volumes: zero.
- Manual builder instances: zero.

No `docker compose up` or container start was performed. `docker compose down --remove-orphans` exited 0, the local Compose image was removed, and the Compose context was deleted.

## Cleanup and residual cache

Final visible Docker inventory contained zero containers, zero images, zero volumes, and only the three default Docker networks. RUN-0013 container/image/network/volume counts were all zero. The owner-only temporary root, release metadata, checksum manifest, marker copies, and both build contexts were removed.

The legitimate `/home/deck/.docker/buildx/` client state was retained. It contained only normal lock, current-selection, activity/defaults directories, and an empty manual-instances directory.

`docker buildx du --builder rootless` reported seven reclaimable cache records totaling **390 bytes**. This is expected BuildKit cache from the two local scratch builds. No Docker or BuildKit prune command was run, and unrelated state was not deleted.

## Docker configuration invariants

| Invariant | Final result |
|---|---|
| Active context | `rootless`, unchanged |
| Rootless socket | `unix:///run/user/1000/docker.sock`, unchanged |
| Rootless security | Present |
| Docker data root | `/home/deck/.local/share/docker` on internal ext4 |
| Storage driver | `fuse-overlayfs`; driver status empty |
| User Docker service | Active |
| Rootful daemon/socket | Absent |
| Daemon configuration SHA-256 | `b58ec579adc94cbb0777e459f25257479febd84927a60b6d098c70326adb6193`, unchanged |
| Docker CLI configuration SHA-256 | `b400a93a05a1e20d83375eb3cab449ff3dd07e05b9a6eaa3acf335633763bce7`, unchanged |
| Manual Buildx builder instances | Zero |
| Registry push/authentication | None |
| Privileged/QEMU/binfmt helpers | None |
| Ports/listeners/network policy | None created or changed |

## Governance files and publication

Created:

- `records/decisions/ADR-0013-user-scoped-docker-buildx-plugin-and-rootless-context-builder.md`
- `docs/tooling/docker-buildx-user-plugin-management.md`
- `records/executions/RUN-0013-install-and-validate-docker-buildx-user-plugin.md`
- `records/validations/VAL-0013-docker-buildx-user-plugin-validation.md`

Updated:

- `README.md`
- `docs/requirements/server-build-sequence-requirements.md`

The pre-commit staged set contained exactly the six intended Markdown paths: four new governance/tooling documents and two narrowly updated current documents. Cached status, stat, name-status, whitespace check, and the complete diff were reviewed. Every object was mode 100644 plain text; the largest was 14,145 bytes. No binary, executable, Buildx asset, checksum manifest, Dockerfile, Compose context, marker, runtime state, secret, credential, private network detail, restricted asset, or unexpected path was staged. No historical RUN/VAL record changed and no lifecycle advancement appeared. `git diff --cached --check` exited 0.

Commit, push, and final refs remain pending in this pre-publication revision.

## Lifecycle and acquisition statement

No AzerothCore source, build, image, database, module, addon, client, client asset, or documentation corpus was acquired. This operation establishes build tooling only. The lifecycle remains **S0 — Proposed**.
