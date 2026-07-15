# AzerothCore Internal Baseline Checkout Management

Status: Authoritative policy at **S0 — Proposed**

## Identity and placement

- Checkout: `CHK-ACORE-WOTLK-001`
- Source: `SRC-ACORE-WOTLK-001`
- Path: `/home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001/`
- Pin: `34a8bd6655c02448d3da6195dcd00647f634dde3`
- Tree: `25ad25f5fb8e119f68fef69b080934f1182ad8d6`
- Authoritative origin: `https://github.com/azerothcore/azerothcore-wotlk.git`

This is an independent full-history clone on internal ext4, not a linked worktree. It has a real `.git` directory and copied objects. The portable verified repository supplies materialization objects; it is not an ongoing object dependency.

## Governed materialization

Create the exact destination only after validating source objects and ext4 semantics. Use a full, single-branch, no-checkout clone with `--no-local` from the verified portable repository. Do not use `--shared`, `--reference`, `--reference-if-able`, hardlinks, alternates, or `git worktree add`. Before checkout, prove pack/index link counts are one, devices/inodes are independent, alternates/promisor/partial-clone state is absent, and strict `git fsck` passes.

Change only the new clone's origin URL to the official upstream without fetching. Preserve the master-only fetch mapping. Then check out the exact approved full SHA with `checkout --detach`.

## Fidelity verification

Require detached HEAD and tree equality, clean status, clean index/file comparisons, unchanged deterministic archive digest, zero missing or unexpected paths, and no LFS or Gitlink dependency. The required symlink must be a real link:

```text
.claude/skills/generate-pr-description
  -> ../../.agents/skills/generate-pr-description
```

Its resolved target must remain inside the checkout. All 42 Git mode-100755 entries must retain owner execution. The self-contained `.git`, common directory, and object directory must all resolve beneath the checkout.

## Clean-baseline protection

Do not edit, patch, format, generate into, or add modules to this checkout. Do not use it as a custom development branch. Do not initialize submodules or LFS, run source hooks/scripts/installers, or place builds, databases, client data, credentials, game assets, or runtime state in it. Future customization belongs in a separately governed path.

## Update, rollback, and removal

A later update must resolve official upstream again, review ancestry and source changes, create/update source and checkout manifests, and repeat independence and fidelity validation. Do not silently advance the existing checkout.

Rollback means selecting a previously validated source/checkout identity, not resetting to a moving branch. Removal requires separate authorization, a clean/no-unique-work check, and deletion of only the exact checkout. Preserve manifests and validation records.

If the portable object repository is unavailable, this clone's copied object database supports local verification. Its official origin supports future recovery only through a separately authorized network operation; no alternates path needs repair.

## Build-context boundary

Static structure passes because the root build context contains required source paths, `.dockerignore` retains `.git`, and the Dockerfile bind mount expects the real `.git` directory. This is not image-build validation. A later operation must separately govern Buildx/Compose invocation, external base images and packages, cache/output storage, credential handling, client-data separation, and cleanup.

No WoW client, Blizzard asset, map, DBC, VMap, MMap, addon, patch, credential, or proprietary payload may be added to this checkout.
