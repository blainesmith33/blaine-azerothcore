# Build Manifest: BLD-ACORE-WOTLK-001

## Identity

| Field | Value |
|---|---|
| Build identifier | `BLD-ACORE-WOTLK-001` |
| Build window | 2026-07-14 23:30:10 through 2026-07-15 00:00 America/Phoenix |
| Source identifier | `SRC-ACORE-WOTLK-001` |
| Checkout identifier | `CHK-ACORE-WOTLK-001` |
| Source pin | `34a8bd6655c02448d3da6195dcd00647f634dde3` |
| Tree | `25ad25f5fb8e119f68fef69b080934f1182ad8d6` |
| Source archive SHA-256 | `26375e7744b7b767c628c3bbaba159e919b04947b647e0dfc7ba5bc77d2556b1` |
| Checkout | `/home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001/` |
| Dockerfile | `apps/docker/Dockerfile` |
| Dockerfile SHA-256 | `5a43e93dd29b10efdf09807517aa782ba7256ce1870039b1a2f94bbcf33c7b6e` |
| Docker Engine | 29.6.1, rootless |
| Docker Compose | v5.3.1; installed but not used |
| Docker Buildx | v0.35.0 |
| Builder / driver | `rootless` / `docker` |
| BuildKit | v0.31.1 |
| Host | SteamOS, Linux x86_64 |
| Platform | `linux/amd64` |
| Storage driver | `fuse-overlayfs` |

## Base image

| Field | Value |
|---|---|
| Repository / tag | `docker.io/library/ubuntu:24.04` |
| Pin used | `24.04@sha256:4fbb8e6a8395de5a7550b33509421a2bafbc0aab6c06ba2cef9ebffbc7092d90` |
| OCI index digest | `sha256:4fbb8e6a8395de5a7550b33509421a2bafbc0aab6c06ba2cef9ebffbc7092d90` |
| Linux AMD64 child manifest | `sha256:52df9b1ee71626e0088f7d400d5c6b5f7bb916f8f0c82b474289a4ece6cf3faf` |
| Authentication | Anonymous; no registry login |
| Package source | Official Ubuntu Noble, updates, security, and backports repositories |
| Package manifest SHA-256 | `dd4bba3197563ab59ed7289931085d6d918f053704f34ceeac8e87ce88902de5` for each final image |
| Package count | 114 packages per final image |

The source and base manifests are pinned. Package versions are acquisition-time repository results; this is not a fully hermetic build.

## Common build arguments

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

All builds used `--builder rootless`, `--platform linux/amd64`, direct target selection, `--pull`, `--load`, plain progress, no provenance/SBOM export, no external cache, and no push.

## Final images

| Target | Tag | Image ID | Created | Size | User | Command | Result |
|---|---|---|---|---:|---|---|---|
| `worldserver` | `blaine-azerothcore-worldserver:src-34a8bd6655c0-clean` | `sha256:0ff979e8b64792e5e5e9a36b6a87eb6f0e91b7ab993996d2f4639c0722ec6ac4` | `2026-07-14T23:55:45.130704372-07:00` | 534,238,211 bytes | `acore` | `worldserver` | PASS |
| `authserver` | `blaine-azerothcore-authserver:src-34a8bd6655c0-clean` | `sha256:a4c91cbe31056ec7d8250af5016fc0e80e8fb5d595a7c3931ef388d89fabc4b8` | `2026-07-14T23:56:27.911150578-07:00` | 231,143,651 bytes | `acore` | `authserver` | PASS |
| `db-import` | `blaine-azerothcore-db-import:src-34a8bd6655c0-clean` | `sha256:03369d90c91e85e6f34970affd3b060dbe3122d4317c185bd7a6f169952fd4aa` | `2026-07-14T23:56:46.196347974-07:00` | 1,073,804,146 bytes | `acore` | `/azerothcore/env/dist/bin/dbimport` | PASS; not run |
| `tools` | `blaine-azerothcore-tools:src-34a8bd6655c0-clean` | `sha256:07791f43c38a593e419f559643501d339fdaebb26d46b9fd205e6eef07891bc3` | `2026-07-14T23:56:58.041177135-07:00` | 246,642,211 bytes | `acore` | none | PASS; tools not run |

All four images are Linux AMD64, have no exposed image ports, no repository digest because they remain local, and carry the exact required source/build/checkout/clean-baseline labels. Authserver and worldserver set `AC_UPDATES_ENABLE_DATABASES=0`. All use the upstream entrypoint and non-root user.

## Compiled outputs

| Image | Path | SHA-256 | Size | Dependency result |
|---|---|---|---:|---|
| authserver | `/azerothcore/env/dist/bin/authserver` | `f209c79658d754b217a54271b1dbc9edf308a5889c12cc222b8cad17c80f0892` | 17,181,040 | PASS; zero unresolved |
| worldserver | `/azerothcore/env/dist/bin/worldserver` | `ff9becd8641591e4f0f1b8ea2b64312d6e6052b26f3cef4666a47edb86a7c05f` | 320,275,600 | PASS; zero unresolved |
| db-import | `/azerothcore/env/dist/bin/dbimport` | `4a97c24c3ec9d3ed8fc41694c0227795957c204d5cc7d9257e38e631ee768a71` | 13,079,016 | PASS; zero unresolved |
| tools | `/azerothcore/env/dist/bin/map_extractor` | `5b1a212cd0d7b4d34c696f1cbe9ea7c3fd7548e0462235b3ca3ac144f7760680` | 2,810,880 | PASS; zero unresolved |
| tools | `/azerothcore/env/dist/bin/mmaps_generator` | `776255ff8769386e3113044e105d5d05637fb55e5b3137a88d3dff25bc108188` | 16,430,992 | PASS; zero unresolved |
| tools | `/azerothcore/env/dist/bin/vmap4_assembler` | `3907bcfe3caf910594f229278add0483effa27207e5e187317d08272c579a5aa` | 7,451,464 | PASS; zero unresolved |
| tools | `/azerothcore/env/dist/bin/vmap4_extractor` | `56bd4b36b72bbb1441775eccbfc6a84b4e0354c74ca5160ac242e9c664930cb4` | 5,986,264 | PASS; zero unresolved |

Each is a regular mode-0755 `acore:acore` x86-64 ELF64 PIE executable with nonzero size. Inspection used `sha256sum`, `ldd`, host `file`, and `readelf`; no AzerothCore binary was executed. Configuration templates present are `authserver.conf.dist`, `worldserver.conf.dist`, and `dbimport.conf.dist`; no active configuration was generated.

## Containment and evidence

- Client-data paths in all images contain zero files; no `client-data` image, downloader execution, ChromieCraft/client, DBC, map, VMap, MMap, or Camera output exists.
- The db-import image contains 7,355 source-controlled SQL files, but no database, importer, or migration was run.
- No MySQL image, container, volume, operation network, server/importer/extractor process, listener change, published port, registry push/authentication, Compose invocation, privileged helper, or extra builder exists.
- Build-log endpoints are limited to official Docker Hub Ubuntu resolution and `archive.ubuntu.com` / `security.ubuntu.com` package access.
- The source checkout remains detached, clean, and equal to the approved pin/tree/archive digest.
- BuildKit cache grew from 390 bytes to 7.431 GB total (1.366 GB shared, 6.065 GB private, 7.431 GB reclaimable) and is intentionally retained.
- Evidence root: `/home/deck/.local/share/blAIne/azerothcore/build-evidence/BLD-ACORE-WOTLK-001/`.
- Evidence files: 71; size: 576,475 bytes.
- `evidence-sha256.txt`: 70 verified entries; SHA-256 `ffac7c4bbf01a3b26dd58d08150627a831ed71ecb64cf10c3712352d0878cb25`.
- Evidence content audit: zero secret indicators, private network addresses, binary payloads, and restricted payload files. A Buildx host-gateway address was replaced with a type-only redaction marker.

## Status boundary

Image build status: **PASS — four approved clean local images built and inspected**. Runtime status: **NOT STARTED / NOT VALIDATED**. No image was pushed; no client data, ChromieCraft, database initialization, service startup, or port publication occurred. The next validation boundary is separately governed client acquisition, followed later by extractor validation against an approved protected client copy. Lifecycle remains **S0 — Proposed**.
