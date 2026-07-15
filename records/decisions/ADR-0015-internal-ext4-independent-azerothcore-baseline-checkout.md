# ADR-0015: Internal ext4 Independent AzerothCore Baseline Checkout

- Status: Accepted at S0 — Proposed
- Date: 2026-07-14
- Scope: Symlink-faithful, self-contained working checkout for the clean baseline
- Related decisions: `ADR-0013`, `ADR-0014`
- Related evidence: `SRC-ACORE-WOTLK-001`, `CHK-ACORE-WOTLK-001`, `RUN-0015`, `VAL-0015`

## Context

ADR-0014 acquired and verified official full-history source objects for `SRC-ACORE-WOTLK-001`, but the portable exFAT path could not represent the pinned tree's symbolic link. The portable object repository remains valid provenance, while a build-input working tree requires Linux symlink and executable-mode semantics. The pinned Dockerfile also bind-mounts `.git` into its build stage, so a linked worktree with a plain `.git` pointer file is not accepted without later build evidence.

## Decision

Materialize `SRC-ACORE-WOTLK-001` as an independent, full-history Git clone on internal ext4 storage; copy objects from the verified portable source-object repository using `git clone --no-local`; set the resulting clone's authoritative origin to the official AzerothCore repository; and check out the approved pin in detached-HEAD state with a self-contained `.git` directory.

| Property | Approved value |
|---|---|
| Checkout identifier | `CHK-ACORE-WOTLK-001` |
| Source identifier | `SRC-ACORE-WOTLK-001` |
| Internal path | `/home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001/` |
| Materialization source | Verified portable object repository at `linux/upstream/azerothcore-wotlk/` |
| Authoritative origin | `https://github.com/azerothcore/azerothcore-wotlk.git` |
| Pin | `34a8bd6655c02448d3da6195dcd00647f634dde3` |
| Tree | `25ad25f5fb8e119f68fef69b080934f1182ad8d6` |
| Filesystem | Internal ext4 under `/home/deck` |
| Checkout state | Detached, clean, unmodified |

## Rationale

Internal ext4 is required because it preserves the pinned symlink and executable modes that exFAT could not. An independent clone is used instead of `git worktree add` so `.git` is a real, self-contained directory in the build-context root. `--no-local` is required to copy Git objects rather than create hardlinks. Shared clones, reference clones, object alternates, and hardlink dependencies are prohibited because the checkout must remain usable without consulting the portable repository.

The portable repository is the materialization transport only. The official GitHub repository remains the authoritative upstream identity. Changing the new clone's origin does not fetch or move the approved pin.

## Clean-baseline boundary

The checkout must remain detached at the exact source pin, clean, and free of patches, generated changes, custom modules, database state, client data, build output, and runtime state. It is not a custom development branch. Later modifications require a separate governed source path, branch, module, fork, or repository.

## Structural build-context result

**STRUCTURAL BUILD-CONTEXT PREREQUISITES PASS.** The checkout contains all source-copy and root-context paths referenced by the pinned Compose/Docker definitions. `.git` is a real internal directory containing the pin, refs, and independent objects; `.dockerignore` does not exclude it; and the Dockerfile's `source=.git` BuildKit mount expects a directory. Symlink and executable modes are faithful. Local-clone provenance appears only in reflog text and creates no object/configuration dependency.

This classification does not validate Docker Buildx, Compose interpolation, image inputs, compilation, cache behavior, external downloads, image contents, databases, client data, or runtime services.

## Update procedure

Do not move this checkout by fetching or checking out a moving branch. A source update requires a new governed source resolution, commit-range review, manifest update, object verification, and preferably a separately identified checkout. The existing checkout remains the immutable comparison baseline until replacement is explicitly approved.

## Removal and recovery

Removal requires separate authorization and must first prove exact path, identifier, official origin, detached pin, clean status, and absence of unique work. Remove only the checkout and only remove operation-created parent directories if empty. Preserve governance records and the portable object repository.

Because objects are copied, the checkout remains verifiable and usable for read-only Git operations if the portable drive is unavailable. Recovery may use its self-contained `.git` database and official origin, but any network fetch, replacement clone, or pin change requires separate authorization.

## Capacity and relationships

The checkout consumes approximately 2.2 GiB total: about 880 MiB working tree and 1.3 GiB `.git`. More than 1.1 TiB remained available on internal ext4 after creation. ADR-0014 continues to govern official source identity and the historical exFAT failure. ADR-0013 continues to govern the rootless Buildx toolchain. Neither decision authorizes an image build here.

## Deferred operations and lifecycle

Actual clean-baseline image building, image pulls, package acquisition, cache governance, database initialization, client-data acquisition, service startup, and network validation remain separate operations. Lifecycle remains **S0 — Proposed**.
