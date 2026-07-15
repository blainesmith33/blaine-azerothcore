# Source Manifest: SRC-ACORE-WOTLK-001

## Identity and provenance

| Field | Value |
|---|---|
| Identifier | `SRC-ACORE-WOTLK-001` |
| Classification | Official third-party open-source upstream; clean AzerothCore WotLK baseline candidate |
| Organization/repository | `azerothcore/azerothcore-wotlk` |
| Official clone URL | `https://github.com/azerothcore/azerothcore-wotlk.git` |
| Verified origin URL | `https://github.com/azerothcore/azerothcore-wotlk.git` |
| Branch observed | `master` |
| Local acquisition time | `2026-07-14T22:18:21-07:00` (America/Phoenix) |
| UTC acquisition time | `2026-07-15T05:18:21+00:00` |
| Research reference | `81a8c779bd16417d6b69390060777b3db7a9f703` |
| Acquisition pin | `34a8bd6655c02448d3da6195dcd00647f634dde3` |
| Upstream head after acquisition | `34a8bd6655c02448d3da6195dcd00647f634dde3` at `2026-07-15T05:28:46+00:00` |
| Upstream advancement | Yes; the research reference is a verified ancestor of the pin |
| Tree | `25ad25f5fb8e119f68fef69b080934f1182ad8d6` |
| Parent | `f8b4e180a5d9f9fe4e50d7d6af0bcdaeeec1bd98` |
| Author/date | Kitzunu; `2026-07-15T05:13:13+02:00` |
| Committer/date | GitHub; `2026-07-15T00:13:13-03:00` |
| Subject | `fix(Core/Calendar): correct malformed mail subject when deleting a ca… (#26615)` |
| Signature state | Signature present but locally unverifiable: Git `%G? = E`; RSA key `B5690EEEBB952194`; public key unavailable |

## Clone and filesystem state

| Field | Value |
|---|---|
| Strategy | `--branch master --single-branch --no-checkout`; full history |
| Fetch mapping | `+refs/heads/master:refs/remotes/origin/master` |
| Shallow | No |
| Partial/promisor clone | No |
| Object verification | PASS: `git fsck --full --strict`, exit 0 |
| Pin ancestry | PASS: pin is `origin/master`; research reference is its ancestor |
| Checkout path | `/run/media/deck/blAIneXPLAT/azerothcore/linux/upstream/azerothcore-wotlk/` |
| Filesystem/mount | `/dev/sda1`, exFAT, read/write |
| Parent Git eligibility | Ignored by exact governed rule; not tracked or staged |
| Detached HEAD | Not completed; clone remains no-checkout because fidelity gate failed |
| Physical working-tree inventory | Zero source files and zero checkout subdirectories; Git object database only |
| `.git` size | Approximately 1.3 GiB |

## Deterministic identity and key files

- Deterministic `git archive --format=tar <pin>` SHA-256: `26375e7744b7b767c628c3bbaba159e919b04947b647e0dfc7ba5bc77d2556b1`.
- Root license blob/SHA-256: `ecbc0593737be657aef92e3adcf3bcbd6fdb812e` / `f9c375a1be4a41f7b70301dd83c91cb89e41567478859b77eef375a52d782505`.
- `.dockerignore`: `bdc121d6c88c5afe73faf03b8150232bc4409a20e7ddafb520dbd5b71afa6f55`.
- `docker-compose.yml`: `443933b550d10bc4a921f1a4505e35fd5152885a8d7448efa31df1eec4f105b5`.
- `apps/docker/Dockerfile`: `5a43e93dd29b10efdf09807517aa782ba7256ce1870039b1a2f94bbcf33c7b6e`.
- `apps/docker/Dockerfile.dev-server`: `b065131aac04c1314402b43017ed5a34739b3e42f379a395cc943f49978973dd`.
- `acore.sh`: `7685c252f30c74f71d5dfc5d48d6ebbd534a05a7914610bd9a01037863dc31dc`.
- `CMakeLists.txt`: `cc30895abcc532a712f21d20766cacfe59365f1cb9ba7ca934430b845b8f7c94`.
- `install.sh`: `71e6d59b853bf827488fefdca202f1bfc4ed2b14fc5d641fc1808ba6c3ad9e47`.
- `.gitattributes`: blob `f4cd590bbe4c8447b29458ac8e6fc7f098bc87a9`, SHA-256 `c131c7345382c8fc7931f40bf799b5eb19802932436b09c9cfa05f787d2be1bc`.
- Root `README.md`: absent at this pin.
- `.gitmodules`: absent.

## Logical pinned-tree inventory

| Property | Result |
|---|---|
| Tracked paths/files | 10,011 |
| Logical directories | 464 |
| Logical blob bytes | 897,467,746 |
| Regular non-executable files | 9,968 |
| Executable files | 42 |
| Symbolic links | 1 |
| Gitlinks/submodules | 0 |
| LFS filters/pointers | 0 detected |
| Case-fold collisions | 0 |
| exFAT-incompatible path names | 0 |
| Longest path | 104 bytes: `src/server/scripts/EasternKingdoms/BlackrockMountain/BlackrockDepths/boss_high_interrogator_gerstahn.cpp` |
| License | GNU General Public License version 2 root text; bundled dependency licenses also present |

The symbolic link is `.claude/skills/generate-pr-description` -> `../../.agents/skills/generate-pr-description`. An immediately removed probe on the authoritative mount failed with `Operation not permitted`; checkout fidelity is therefore **DEFERRED/FAIL at this path**. No case or filename collision independently blocks checkout.

The largest logical blobs are SQL source data. The largest is `data/sql/base/db_world/broadcast_text_locale.sql` at 76,428,259 bytes. These are upstream source-controlled database definitions, not an initialized database or runtime backup.

## Build and modification status

- Source-object acquisition: **PASS**.
- Source checkout/build readiness: **DEFERRED** because the exFAT mount cannot faithfully represent the pinned symlink.
- Source status: verified, full-history, official objects retained in no-checkout form.
- Modification status: no source file was checked out or modified; no module was added.
- Build status: not built or build-validated.
- Database status: not initialized or imported.
- Client-data status: not acquired.
- Service status: no container, authserver, worldserver, database, or listener started.

The next validation boundary is a separately governed, symlink-faithful checkout location or mechanism, followed by detached-HEAD and clean-working-tree verification. No compatibility claim extends beyond source-object integrity and static pinned-tree inspection.

## Checkout materialization — 2026-07-14

The historical exFAT result above remains valid for the portable object-repository path. A separate materialization completed under ADR-0015:

| Field | Result |
|---|---|
| Checkout identifier | `CHK-ACORE-WOTLK-001` |
| Internal path | `/home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001/` |
| Filesystem | Internal ext4, read/write |
| Clone independence | PASS: `--no-local`; copied pack; no alternates, hardlinks, partial/promisor state, or external common/object directory |
| Authoritative origin | Official AzerothCore HTTPS URL |
| Detached pin | PASS: `34a8bd6655c02448d3da6195dcd00647f634dde3` |
| Tree | PASS: `25ad25f5fb8e119f68fef69b080934f1182ad8d6` |
| Symlink fidelity | PASS: one real link with exact text and in-root target |
| Executable fidelity | PASS: all 42 Git executable entries retain owner execution |
| Self-contained `.git` | PASS: real internal directory with copied objects and refs |
| Working-tree cleanliness | PASS: zero modified, missing, untracked, or ignored paths |
| Deterministic archive digest | PASS: unchanged |
| Structural build context | `STRUCTURAL BUILD-CONTEXT PREREQUISITES PASS` |
| Actual image build | Not run; not validated |

Source checkout fidelity is now established only at the internal checkout path. The portable path remains a no-checkout evidence/object repository. No source was modified, no module was added, no image was built or pulled, no database was initialized, no client data was acquired, and no service was started. The next boundary is a separately governed clean-baseline image build.
