# RUN-0015 — Establish Internal ext4 AzerothCore Baseline Checkout

## Record control

- Date: 2026-07-14 (America/Phoenix)
- Verification window: `2026-07-14T22:55:03-07:00` through `2026-07-14T23:00:52-07:00` and subsequent publication work
- Lifecycle before and after: `S0 — Proposed`
- Objective: establish `CHK-ACORE-WOTLK-001` as an independent, symlink-faithful internal ext4 checkout of `SRC-ACORE-WOTLK-001`, validate static build-context structure, publish governance evidence, and stop before build/runtime work.
- Governance root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Portable source-object repository: `/run/media/deck/blAIneXPLAT/azerothcore/linux/upstream/azerothcore-wotlk/`
- Internal checkout: `/home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001/`
- Starting governance commit: `1e74a39cec174d8372f0cc2e0308417205b17023`
- Publication state in this revision: technical validation and governance content complete; commit and push evidence pending normal publication.

## Authorization boundary

Persistent writes were limited to the governance repository, the exact internal checkout and its necessary parents, plus an immediately removed owner-only ext4 probe. No sudo, package/SteamOS change, worktree link, shared/reference clone, hardlink, alternates dependency, source modification, module, submodule, LFS, hook/script execution, Docker/Buildx/Compose build, image pull, container, database, client data, port, server, network change, fork, or upstream push was authorized or performed.

## Governance starting state

- Canonical governance root and `main` branch matched authorization.
- Working tree was clean and no active Git operation existed.
- Origin exactly matched the authorized SSH remote.
- Local HEAD, local `origin/main`, and remote main all equaled `1e74a39cec174d8372f0cc2e0308417205b17023`; expected history was current.
- ADR-0014, RUN-0014, VAL-0014, and `SRC-ACORE-WOTLK-001` existed and preserved the object-PASS/checkout-DEFERRED split.
- Tracked-file count was 92. Governance storage remained read/write exFAT with approximately 953 GiB free. Lifecycle remained S0.

## Portable source-object verification

The portable repository retained official origin, master-only fetch mapping, non-shallow/non-partial status, pin commit `34a8bd6655c02448d3da6195dcd00647f634dde3`, tree `25ad25f5fb8e119f68fef69b080934f1182ad8d6`, and one pack with 210,174 objects. `git fsck --full --strict` exited 0. No garbage, alternates, or promisor configuration appeared.

The streamed `git archive` digest remained `26375e7744b7b767c628c3bbaba159e919b04947b647e0dfc7ba5bc77d2556b1`. Official `HEAD` and `master`, observed at `2026-07-15T05:55:03+00:00`, still equaled the approved pin. No fetch or pin update occurred.

## Internal filesystem and capacity preflight

The destination and its new parent chain were absent initially. `/home/deck` resolved to `/dev/nvme0n1p8[/deck]`, ext4, read/write, owned by `deck`; 1,168,697,995,264 bytes (about 1.1 TiB) were available, exceeding the 5 GiB gate.

An owner-only mode-0700 probe on the same filesystem verified and immediately removed:

- symbolic link creation and exact `readlink` text;
- mode 0755 persistence and executable access;
- coexistence of two case-distinct filenames;
- an ordinary AzerothCore-style filename; and
- a 125-byte relative path, exceeding the pinned 104-byte longest path.

Probe residue count was zero. Only `blAIne/azerothcore/checkouts` parents were created, all `deck:deck`, mode 0755, real directories, and canonically beneath `/home/deck/.local/share/`. The final destination was still absent immediately before clone.

## Independent clone and transfer

Executed exactly:

```text
git clone --no-local --branch master --single-branch --no-checkout /run/media/deck/blAIneXPLAT/azerothcore/linux/upstream/azerothcore-wotlk /home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001
```

The clone completed once with exit 0; no competing verification or second clone occurred during transfer. Before origin change, origin recorded the local materialization path, fetch scope remained master-only, shallow=false, partial/promisor state absent, `origin/master` equaled the pin, and strict full object verification passed. The independent pack contained 210,174 objects and was approximately 1.23 GiB.

## Independence evidence

- `.git` was a real directory.
- `.git/objects/info/alternates` and `.git/commondir` were absent.
- No alternates, partial-clone, or promisor configuration existed.
- Internal pack, index, and reverse-index files were regular files on device 66312, each link count 1.
- Portable pack files were on device 2049, each link count 1; names/inodes/devices differed.
- Git directory, common directory, and object directory all resolved inside the internal checkout.

No shared-object or hardlink dependency was detected. Independence passed before origin change and checkout.

## Authoritative origin and Git configuration

The new clone's origin was changed, without fetch, to `https://github.com/azerothcore/azerothcore-wotlk.git`. Fetch/push output and local config showed only that official origin; master-only fetch mapping remained.

`core.filemode=true`; `core.symlinks` and `core.ignorecase` were unset with positive ext4 semantic evidence and later faithful checkout evidence. No clean/smudge filter, sparse checkout, custom active hook, Gitlink, `.gitmodules`, LFS pointer, partial-clone setting, promisor remote, or external object control was present. No configuration value was silently overridden.

## Detached checkout and fidelity

`git checkout --detach 34a8bd6655c02448d3da6195dcd00647f634dde3` exited 0. HEAD and tree equaled the approved values. Branch status was `## HEAD (no branch)`. `git diff --check`, `diff-files --quiet`, and `diff-index --quiet HEAD --` passed; describe returned `v1.0.1-18266-g34a8bd665`; no untracked/ignored path existed.

The blocking link `.claude/skills/generate-pr-description` was a real filesystem symbolic link, Git mode 120000, with exact text `../../.agents/skills/generate-pr-description`. It resolved to `.agents/skills/generate-pr-description` inside the checkout and its target existed.

All 42 Git mode-100755 entries retained owner execution. All 9,968 mode-100644 entries had no owner-executable mismatch. There were zero missing tracked paths, 10,011 tracked paths total, 464 physical directories, 10,010 regular files, one symlink, zero Gitlinks, zero LFS requirements, and zero case-fold collisions. The longest path remained 104 bytes.

Working-tree apparent bytes were 897,467,746 (about 880 MiB via `du -sh`), `.git` approximately 1.3 GiB, and total checkout 2,229,925,046 bytes (about 2.2 GiB). Final internal free space was 1,166,446,243,840 bytes. The 20 largest files matched the pinned logical SQL inventory. The streamed archive SHA-256 again matched the approved value.

One inventory pipeline's `sort` reported a harmless broken pipe after `head` consumed the required 20 lines; subsequent counts and digest passed, and no state changed.

## Self-contained Git structure

`.git`, common directory, and object directory all resolved inside the checkout. `.git` was a directory, not a linked-worktree pointer. No `commondir`, alternates file, external object directory, or operational absolute host path was configured. Clone-source absolute paths remained only in reflog provenance text; they do not affect ordinary object lookup.

## Static build-context review

Classification: **STRUCTURAL BUILD-CONTEXT PREREQUISITES PASS**.

- Root Compose build contexts are `.` and referenced Dockerfiles exist.
- Required `CMakeLists.txt`, `conf`, `deps`, `src`, `modules`, `apps`, and Docker/Compose paths exist.
- `.dockerignore` does not omit `.git`.
- `apps/docker/Dockerfile` bind-mounts `source=.git` to `/azerothcore/.git`; the source is a real self-contained directory.
- Pin, refs, tags, and objects are available to the CMake revision commands (`git describe`, `git show`, and `git rev-parse`).
- The single symlink and executable modes are faithfully present for a future context transfer.

No build, Compose interpolation, external image/package acquisition, compilation, cache creation, or image inspection was performed. Actual image-build readiness remains a later validation boundary.

## Docker and Buildx invariants

An initial restricted-namespace read-only check could not access the user socket or user bus and reported permission denied. It changed no state. The check was repeated from the authoritative host context and passed: Docker Engine 29.6.1, Compose v5.3.1, RootlessKit 3.0.1, Buildx v0.35.0 with the approved checksum, active `rootless` context, running rootless Docker-driver builder/BuildKit v0.31.1, rootless security, internal data root, and `fuse-overlayfs`. Rootful services/socket remained absent. Visible images, containers, and volumes were zero; only default bridge/host/none networks existed.

## Governance files

Created:

- `records/decisions/ADR-0015-internal-ext4-independent-azerothcore-baseline-checkout.md`
- `docs/source/azerothcore-internal-baseline-checkout-management.md`
- `manifests/checkouts/CHK-ACORE-WOTLK-001.md`
- this RUN record
- `records/validations/VAL-0015-internal-ext4-azerothcore-baseline-checkout-validation.md`

Updated current documents: source manifest, official-source management, root README, and server build-sequence requirements. ADR-0014, RUN-0014, and VAL-0014 historical evidence was not changed.

## Actions explicitly not performed

No source edit, patch, format, generated file, module, submodule, LFS, hook/script, `acore.sh`, CMake, compiler, installer, Docker/Buildx/Compose build, image pull, registry authentication, container, database, client data, map/DBC/VMap/MMap, client/addon/patch/corpus, port, service, network configuration, upstream fetch/push, fork, or branch was performed.

## Publication and lifecycle

Staged audit, commit, normal push, and final refs remain pending. Technical checkout and structural validation pass, but VAL-0015 must not be marked final PASS until governance publication and remote verification complete. Lifecycle remains **S0 — Proposed**.
