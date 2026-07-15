# RUN-0016 — Build Clean Unmodified AzerothCore Baseline Images

## Record control

- Date: 2026-07-14 through 2026-07-15 (America/Phoenix)
- Lifecycle before and after: `S0 — Proposed`
- Objective: build and inspect four clean pinned server/tool images, retain local outputs and governed evidence, publish documentation, and stop before client, database, service, or network deployment work.
- Build identifier: `BLD-ACORE-WOTLK-001`
- Governance root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Source checkout: `/home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001/`
- Evidence root: `/home/deck/.local/share/blAIne/azerothcore/build-evidence/BLD-ACORE-WOTLK-001/`
- Starting governance commit: `43c22db051fd2fa2117e614fd29025b4eff92ed8`
- Publication state in this revision: technical validation, evidence sealing, staged audit, first governance commit, normal push, and first post-push ref verification pass; this completed evidence is ready for the single validation-only follow-up commit.

## Authorization boundary

Writes were limited to rootless Docker/BuildKit state, the exact internal evidence path, and the intended governance Markdown files. The pinned checkout was read-only in practice and remained clean. No sudo, host package, SteamOS, Docker configuration/context/builder/storage/network, firewall, router, DNS, VPN, proxy, tunnel, ingress, Compose, QEMU, privileged helper, source modification, module, client, database, service, listener, registry authentication, or image push was authorized or performed.

## Governance and source preflight

The governance repository was clean on `main`, had no active Git operation, used the authorized origin, and local HEAD/origin/main/remote main all equaled the expected starting commit. Required ADR/RUN/VAL-0015 and source/checkout manifests existed; lifecycle remained S0.

The checkout retained official origin, detached exact pin `34a8bd6655c02448d3da6195dcd00647f634dde3`, tree `25ad25f5fb8e119f68fef69b080934f1182ad8d6`, and archive SHA-256 `26375e7744b7b767c628c3bbaba159e919b04947b647e0dfc7ba5bc77d2556b1`. Strict fsck, index/files diffs, self-contained `.git`, no-alternates/no-hardlinks/no-partial state, exact symlink, 42 executable modes, and zero untracked/ignored source paths passed.

## Pinned build definition

Static inspection confirmed `apps/docker/Dockerfile`, final target names, `APPS_BUILD=all`, configurable tools build, source COPY paths, `.git` bind, four extractor tools, distinct client-data downloader target, and absence of a blAIne-added module. `.dockerignore` retained `.git`.

Hashes:

- Dockerfile: `5a43e93dd29b10efdf09807517aa782ba7256ce1870039b1a2f94bbcf33c7b6e`
- `docker-compose.yml`: `443933b550d10bc4a921f1a4505e35fd5152885a8d7448efa31df1eec4f105b5`
- `.dockerignore`: `bdc121d6c88c5afe73faf03b8150232bc4409a20e7ddafb520dbd5b71afa6f55`
- `CMakeLists.txt`: `cc30895abcc532a712f21d20766cacfe59365f1cb9ba7ca934430b845b8f7c94`

GoogleTest external-source logic exists in pinned test CMake definitions, but `BUILD_TESTING` defaults off; logs confirmed it did not execute.

## Host and Docker preflight

- User: `deck`, UID/GID 1000; Linux x86_64; 8 logical CPUs.
- Memory: 10,441,668 kB available; swap: 5,389,108 kB free; combined gate exceeded 8 GiB.
- Internal storage: about 1.1 TiB free, exceeding the 100 GiB gate.
- Docker Engine: client/server 29.6.1, active rootless context and socket.
- Compose: v5.3.1, installed but not invoked.
- Buildx: v0.35.0; existing `rootless` Docker-driver builder; BuildKit v0.31.1.
- Storage: `fuse-overlayfs`, data root `/home/deck/.local/share/docker` on internal ext4.
- Rootful Docker absent; no extra manually created builder.
- Starting state: zero containers, images, volumes, target tags, or non-default networks; BuildKit cache 390 bytes.
- Docker daemon/CLI configuration hashes were recorded and remained unchanged.

Read-only `docker buildx ls` continued to warn that the unused builder entry named `default` cannot reach the absent rootful socket. This is the previously governed, expected default-context condition; the active `rootless` builder remained running and was the only builder used. Docker also retained previously observed rootless cgroup warnings for unsupported cpuset and selected I/O controls. Neither warning changed build scope or the validated CPU/memory/PID controller boundary.

## Evidence directory and base image

The previously absent evidence root was created on internal ext4 as `deck:deck`, mode 0700, with log, inspect, history, package, binary, resource, and base-image subdirectories.

Anonymous official inspection resolved `docker.io/library/ubuntu:24.04` to OCI index `sha256:4fbb8e6a8395de5a7550b33509421a2bafbc0aab6c06ba2cef9ebffbc7092d90` and Linux AMD64 child manifest `sha256:52df9b1ee71626e0088f7d400d5c6b5f7bb916f8f0c82b474289a4ece6cf3faf`. No registry credential existed or was added. Every build used `24.04@sha256:4fbb8e6a8395de5a7550b33509421a2bafbc0aab6c06ba2cef9ebffbc7092d90`.

## Build execution

All builds used the common arguments and labels recorded in the build manifest, the `rootless` builder, native Linux AMD64, direct Dockerfile target, `--pull`, `--load`, plain progress, and disabled provenance/SBOM export. No Compose, push, external cache, host networking, or privileged mode was used.

| Order | Target | Window (America/Phoenix) | Duration | Exit | Image ID | Cache behavior |
|---:|---|---|---:|---:|---|---|
| 1 | worldserver | 23:30:10–23:55:52 | 1,542 s | 0 | `sha256:0ff979e8...ec6ac4` | Full dependency/configure/compile/install/export; 0 cached-step markers |
| 2 | authserver | 23:56:27–23:56:29 | 2 s | 0 | `sha256:a4c91cbe...fabc4b8` | 15 shared stages cached; final target copied/exported |
| 3 | db-import | 23:56:29–23:56:55 | 26 s | 0 | `sha256:03369d90...2fd4aa` | 15 shared stages cached; source SQL copied/exported |
| 4 | tools | 23:56:55–23:57:00 | 5 s | 0 | `sha256:07791f43...891bc3` | 15 shared stages cached; four tools copied/exported |

Clang 18.1.3 configured both C and C++ compilation. CMake configure/generate, all seven required binary targets, install, and four image exports succeeded.

### Failed attempts and corrections

1. The first worldserver command exited 127 before Docker execution because the escalated host shell did not include `/home/deck/bin` in `PATH`. No registry access, BuildKit work, or image occurred. Its log and metadata were preserved. The corrected command used an operation-local PATH and exact `/home/deck/bin/docker`; no profile changed.
2. One convenience image-inspect Go template failed because the tools image legitimately lacks a `Cmd` key. Raw inspect JSON plus an independent parser replaced the template; image validation was unaffected.
3. An evidence-summary shell expression had a quoting error before producing a usable report; the corrected read-only scan passed.
4. The first package format was over-escaped and produced blank records. Manifests were recaptured with `dpkg-query -W`, verified as 114 package/version rows each, and the evidence set was resealed before governance PASS.
5. A naive private-IP regex treated versions/timestamps as addresses. An IP-aware parser was used instead. It found one real Buildx host-gateway field, which was replaced in retained evidence with a type-only marker; the final audit reports zero private addresses.

## Acquisition and build-log containment

Observed acquisition was limited to official Docker Hub Ubuntu resolution plus `archive.ubuntu.com` and `security.ubuntu.com`. No executed reference to the client-data downloader, `wowgaming/client-data`, ChromieCraft, `git clone`, module catalogue, MySQL image, database service, or Compose startup appeared.

## Image and binary inspection

Raw inspect JSON and history were retained for all four images. Each is Linux AMD64, `acore`, correctly labeled, local-only, and has no exposed image port. Authserver/worldserver database updates are disabled. The tools image correctly has no default command.

Hardened `/bin/sh` inspections used network none, read-only root, all capabilities dropped, no-new-privileges, no explicit mounts, and automatic removal. The seven expected binaries passed path, executable mode, owner, nonzero-size, SHA-256, and `ldd` checks with zero unresolved dependencies. No AzerothCore binary was executed. Because final images do not contain `file`/`readelf`, stopped never-started containers were used to copy binaries temporarily for host ELF inspection; every copy/container/automatic anonymous volume was removed. All binaries were ELF64 x86-64 PIE files.

All final images share a corrected 114-row package manifest with SHA-256 `dd4bba3197563ab59ed7289931085d6d918f053704f34ceeac8e87ce88902de5`. Exact binary hashes are in the build manifest and sealed evidence.

## Client, database, service, and cleanup containment

Every inspected client-data directory had zero files. No client-data image, ChromieCraft path, WoW client, DBC, map, VMap, MMap, Camera output, or downloader execution was found. The db-import image contains 7,355 source-controlled SQL files; no database or importer was run.

Final state: zero containers, volumes, operation networks, server/importer/extractor processes, listener changes, client/MySQL/dev images, registry pushes, and published ports. Exactly four approved images remain. Docker context/configuration, builder, rootless security, and storage driver remain unchanged. The source remains detached and clean at the approved identity.

BuildKit cache is intentionally retained: 7.431 GB total, 1.366 GB shared, 6.065 GB private, all reported reclaimable. No prune was run.

## Evidence integrity

The evidence set contains 71 files and 576,475 bytes. `evidence-sha256.txt` covers 70 other evidence files and verifies without error. Its SHA-256 is `ffac7c4bbf01a3b26dd58d08150627a831ed71ecb64cf10c3712352d0878cb25`. Content scanning found zero secrets, private network addresses after type-only sanitization, binary payloads, restricted payloads, source archives, or image exports.

## Governance publication

Created: ADR-0016, build-management documentation, `BLD-ACORE-WOTLK-001`, RUN-0016, and VAL-0016. Updated: README and server-build sequence requirements. No historical record changed.

The staged set contained exactly eight intended text files: five new governed Markdown records/documents plus `.gitignore`, README, and the current server-build requirements. New files were mode 100644. `git diff --cached --check`, full diff review, file-type/size inspection, and staged secret/private-network/binary/restricted-content scans passed. No evidence file, source checkout, binary, image, cache, client asset, database, credential, private network detail, or historical RUN/VAL record was staged.

Commit `a3d8ec9b1321a0b7017972d1ec98d416ef1582af` (`Build clean AzerothCore baseline images`) was created and pushed normally as a fast-forward from `43c22db051fd2fa2117e614fd29025b4eff92ed8`. No force or history rewrite occurred. Immediately after the push, local HEAD, local `origin/main`, and remote `refs/heads/main` all equaled `a3d8ec9b1321a0b7017972d1ec98d416ef1582af`; starting history remained an ancestor.

This record and VAL-0016 are completed by the single authorized validation-only follow-up commit. That commit cannot contain its own object ID; its final local/origin/remote equality is captured by the operation's final verification and final report.

No ChromieCraft/client, pre-extracted data, database initialization, server startup, port publication, image push, Compose orchestration, privileged helper, or source modification occurred. Lifecycle remains **S0 — Proposed**.
