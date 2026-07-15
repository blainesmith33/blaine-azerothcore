# RUN-0014 — Acquire and Pin Official AzerothCore Source

## Record control

- Date: 2026-07-14 (America/Phoenix)
- Acquisition start: `2026-07-14T22:18:21-07:00` / `2026-07-15T05:18:21+00:00`
- Lifecycle before and after: `S0 — Proposed`
- Objective: acquire and verify official AzerothCore WotLK source objects, pin `SRC-ACORE-WOTLK-001`, statically inventory build/acquisition behavior, publish governance evidence, and stop before build or runtime work.
- Governance root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Source object clone: `/run/media/deck/blAIneXPLAT/azerothcore/linux/upstream/azerothcore-wotlk/`
- Starting governance commit: `38c1e515d47564fe7f04f3d55503785b104ce5a7`
- Publication state in this revision: source/object validation and governance content complete; commit and push evidence pending normal publication.

## Authorization boundary

Writes were limited to the governance repository, the exact ignored upstream clone, and an immediately removed filesystem-representability probe beneath `linux/upstream/`. No sudo, package or SteamOS change, Docker change, build, image pull, container, database, client data, module, submodule, LFS pull, patch, compiler, runtime, port, server, or upstream push was authorized or performed.

## Governance starting state

- Canonical root matched the authorized path; branch `main`; clean working tree; no active Git operation.
- Origin matched `git@github.com:blainesmith33/blaine-azerothcore.git`.
- Local HEAD, local `origin/main`, and remote `refs/heads/main` all equaled the starting commit.
- The expected commit was current and an ancestor; ADR-0013 and VAL-0013 existed; lifecycle references remained S0.
- Tracked files: 86. Governance storage: `/dev/sda1`, exFAT, read/write, approximately 954 GiB available.
- `linux/upstream/`, `manifests/sources/`, and the intended clone did not exist.

## Docker and Buildx invariant check

Read-only checks passed: Docker client/server 29.6.1, Compose v5.3.1, RootlessKit 3.0.1, active `rootless` context, approved user socket, rootless security, `fuse-overlayfs`, and `/home/deck/.local/share/docker` on internal ext4. The selected context-derived `rootless` Docker-driver builder was running with BuildKit v0.31.1. Buildx v0.35.0 remained a regular `deck:deck` mode-0755 plugin with SHA-256 `d41ece72044243b4f58b343441ae37446d9c29a7d6b5e11c61847bbcf8f7dfda`. No rootful service/socket existed. Zero visible images and containers were present. No runtime command was executed beyond inspection.

## Parent protection

Added the exact rule:

```gitignore
# Official upstream source checkouts — governed separately and never vendored
linux/upstream/azerothcore-wotlk/
```

Created `linux/upstream/README.md`. Before cloning, `git check-ignore --no-index` proved the intended clone paths ignored while the directory README remained eligible. After acquisition, `git check-ignore -v` identified only the exact governed rule for both nested `.git/HEAD` and an upstream source path, and `git ls-files` confirmed no upstream path tracked.

## Official upstream verification and acquisition pin

Command, executed twice before clone:

```text
git ls-remote https://github.com/azerothcore/azerothcore-wotlk.git HEAD refs/heads/master
```

Both calls exited 0 and returned `34a8bd6655c02448d3da6195dcd00647f634dde3` for `HEAD` and `refs/heads/master`. The official identity did not redirect to another organization/repository. The research reference `81a8c779bd16417d6b69390060777b3db7a9f703` exists as a commit and is an ancestor, so upstream advanced. A post-acquisition query at `2026-07-15T05:28:46+00:00` still returned the pin.

## Clone, origin, and object verification

Executed:

```text
git clone --branch master --single-branch --no-checkout https://github.com/azerothcore/azerothcore-wotlk.git /run/media/deck/blAIneXPLAT/azerothcore/linux/upstream/azerothcore-wotlk
```

The full transfer was allowed to finish before verification. An early read-only verification raced the active `index-pack` and temporarily saw no refs; process inspection proved the original clone was still active, so no removal or second clone occurred. After exit 0:

- origin fetch/push URL exactly matched official upstream;
- fetch mapping was `+refs/heads/master:refs/remotes/origin/master`;
- `--is-shallow-repository` was false;
- no partial-clone or promisor configuration existed;
- `origin/master` equaled the acquisition pin;
- the pin was a commit and an ancestor of `origin/master`;
- `git fsck --full --strict` exited 0;
- one pack contained 210,174 objects, size approximately 1.23 GiB, with zero garbage.

No garbage collection, repack, submodule, or LFS command was run.

## Tree-fidelity audit and stop result

The pinned tree is `25ad25f5fb8e119f68fef69b080934f1182ad8d6`. Direct Git-tree inspection found 10,011 paths, 9,968 non-executable regular files, 42 executables, one symbolic link, zero Gitlinks, zero case-fold collision groups, zero exFAT-invalid names, no `.gitmodules`, and no LFS pointer/filter declaration.

The symlink is `.claude/skills/generate-pr-description` -> `../../.agents/skills/generate-pr-description`. A disposable exFAT probe attempted `ln -s` and failed with `Operation not permitted`; the ordinary target probe file was removed and no probe residue remained.

This triggered the required checkout-fidelity stop. `git checkout --detach` was not run. The full verified no-checkout object repository was retained. The clone has no physical source files, and the initial no-checkout index/branch representation may display the absent tree as deletions; this is not a modified checkout and must not be used for a build. Source-object acquisition is PASS; checkout/build readiness is DEFERRED; overall source acquisition is not full PASS.

## Pinned commit and deterministic evidence

- Pin: `34a8bd6655c02448d3da6195dcd00647f634dde3`.
- Tree: `25ad25f5fb8e119f68fef69b080934f1182ad8d6`.
- Parent: `f8b4e180a5d9f9fe4e50d7d6af0bcdaeeec1bd98`.
- Author/date: Kitzunu, `2026-07-15T05:13:13+02:00`.
- Committer/date: GitHub, `2026-07-15T00:13:13-03:00`.
- Subject: `fix(Core/Calendar): correct malformed mail subject when deleting a ca… (#26615)`.
- Signature: present but locally unverifiable (`%G? = E`; public key unavailable).
- Streamed deterministic archive SHA-256: `26375e7744b7b767c628c3bbaba159e919b04947b647e0dfc7ba5bc77d2556b1`; no archive was written.
- Root license: GNU GPL version 2 text; blob and content hashes recorded in the manifest.

## Source inventory

Logical pinned tree: 10,011 files/paths, 464 directories, 897,467,746 blob bytes. The object database is approximately 1.3 GiB. The no-checkout physical working tree contains zero source files and no checkout subdirectories. The largest 20 logical files are source-controlled SQL definitions; the largest is 76,428,259 bytes. These are source definitions, not initialized database state.

Forty-two executable entries and the one symlink were inventoried. No Gitlink/submodule or LFS dependency exists. The longest path is 104 bytes and all path names passed exFAT character checks. Exact counts, symlink, key hashes, and license are in the source manifest.

## Static build-definition inventory

No definition was executed. The root `docker-compose.yml` defines seven services: database, database import, worldserver, authserver, client-data initialization, tools, and development server. It defines one application network and four named volumes.

The primary build context is repository root. `apps/docker/Dockerfile` defines skeleton, build, runtime, authserver, worldserver, db-import, client-data, and tools stages; `apps/docker/Dockerfile.dev-server` defines the dev stage. Root CMake enters at `CMakeLists.txt`; additional dependency/server/tool CMake entry points exist. `acore.sh` and `bin/acore` form dashboard/wrapper entry points. Expected generated areas include `var/build`, `var/ccache`, and `env/dist` paths.

Upstream Compose includes environment-overridable host publications whose defaults map database, auth, world, and SOAP service ports. These definitions are inventory only: no exact port is approved for blAIne exposure, no port was published, and the network-boundary ADR remains controlling. Compose contains credential-bearing database connection/default fields and an env-file reference; values were not reproduced in governance records and require later secret governance.

No active `privileged` or host-network declaration and no Compose secret object were found in the root definition. The devcontainer example contains commented Docker-socket/ptrace examples; they are not approved.

## External acquisition behavior identified

- External base/runtime image references include `ubuntu:24.04`, `mysql:8.4`, and locally named `acore/ac-wotlk-*` outputs.
- Dockerfile build stages run Ubuntu `apt-get update/install`, requiring package-network access during a future build.
- `Dockerfile.dev-server` declares an external Dockerfile frontend and installs development packages; it is not selected for the clean production baseline.
- The client-data image installs curl/unzip, then its runtime command invokes `inst_download_client_data`.
- `apps/installer/includes/functions.sh` resolves an official-release default and downloads/extracts `wowgaming/client-data` `data.zip`; this would acquire DBC/map/VMap/MMap-related client data and was not executed.
- The tools profile binds a game-client `Data` path for local extraction; no client path or asset was supplied.
- Module-management code contains GitHub/API/catalog acquisition behavior; no catalog, repository, or module was acquired.
- Installer OS scripts contain apt/brew/wget operations; none was run.
- The database-import stage copies pinned repository SQL and modules into an image and would initialize/import only when later run; no separate database release download was identified in that target, and no database operation occurred.

## Build-compatibility questions deferred

The main Dockerfile uses a BuildKit bind mount of `.git` and copies working-tree paths, so the present no-checkout clone is unusable. A later operation must resolve symlink-faithful placement, exFAT bind/file-mode semantics, approved output/cache storage, explicit versus implicit Compose selection of the already established `rootless` builder, external base-image/package acquisition, expected cache/image size, credential injection, client-data separation, and default publication suppression. Expected image count/size and runtime compatibility remain unvalidated.

## Files created and updated

Created: ADR-0014, this RUN, VAL-0014, the source-management document, source manifest, and `linux/upstream/README.md`. Updated: root `.gitignore`, root `README.md`, and server build-sequence requirements. The ignored upstream clone is an independently governed object repository, not a parent-repository file set.

## Actions explicitly not performed

No source checkout, source modification, module, submodule, LFS pull, archive export, Docker/Buildx/Compose build, image pull, registry authentication, container, database, client data, client, map/DBC/VMap/MMap extraction, compiler, server, port, network, or runtime operation occurred. No upstream push/fork/branch/tag was created.

## Staged audit, publication, and lifecycle

The staged audit, commit, normal push, and final refs will be completed only for the intended Markdown/governance set after source paths remain ignored and all split-result wording is verified. No full validation PASS is claimed. Lifecycle remains **S0 — Proposed**.
