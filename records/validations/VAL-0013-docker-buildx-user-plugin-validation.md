# VAL-0013 — Docker Buildx User-Plugin Validation

## Record control

- Date: 2026-07-14
- Subject: `RUN-0013`
- Branch: `main`
- Status: **PASS — runtime, cleanup, governance publication, and remote-ref verification passed**
- Lifecycle: `S0 — Proposed`

PASS was assigned only after the intended Markdown set was audited, committed, pushed normally, local HEAD, local `origin/main`, and remote `main` matched, starting history remained an ancestor, and post-push runtime/cleanup invariants passed.

## Installation, runtime, and containment assertions

| Assertion | Result | Evidence |
|---|---|---|
| Repository synchronized at start | PASS | Clean `main`; no active Git operation; HEAD, origin/main, and remote main all `311be3d0e5178c8560f7a23da61c0806bea0c59c`. |
| Correct user and architecture | PASS | `deck`, UID 1000; Linux x86-64/AMD64. |
| Official release provenance | PASS | Official `docker/buildx` tag `v0.35.0`; draft=false; prerelease=false; exact official asset present. |
| Checksum manifest | PASS | Exactly one applicable entry for `buildx-v0.35.0.linux-amd64`. |
| Installed checksum | PASS | Expected and installed SHA-256 both `d41ece72044243b4f58b343441ae37446d9c29a7d6b5e11c61847bbcf8f7dfda`. |
| Installed path | PASS | `/home/deck/.docker/cli-plugins/docker-buildx` on internal ext4 storage. |
| Owner and mode | PASS | `deck:deck`, `0755`; regular file. |
| Binary type and architecture | PASS | Static ELF 64-bit Linux x86-64 executable. |
| Plugin discovery and exact version | PASS | Direct binary and `docker buildx version` report v0.35.0, commit `a319e5b...`. |
| Active rootless context | PASS | Context `rootless`; approved user-scoped Unix socket; rootless security option present. |
| Usable rootless builder | PASS | Selected `rootless*`; Docker driver; running; BuildKit v0.31.1; inspect exit 0. |
| Rootful default classified accurately | PASS | Unused `default` entry resolves to absent rootful socket; not treated as an installation failure. |
| No additional builder created | PASS | No buildx create/use/rm command; manual instances directory contains zero files. |
| Docker daemon configuration unchanged | PASS | Daemon JSON SHA-256 remains `b58ec579adc94cbb0777e459f25257479febd84927a60b6d098c70326adb6193`. |
| Docker CLI configuration unchanged | PASS | Config SHA-256 remains `b400a93a05a1e20d83375eb3cab449ff3dd07e05b9a6eaa3acf335633763bce7`. |
| Rootless socket unchanged | PASS | Endpoint remained the approved user socket; no rootful socket appeared. |
| Storage unchanged | PASS | Data root remained internal ext4; storage driver `fuse-overlayfs`; containerd snapshotter driver status absent. |
| Direct scratch build | PASS | `--builder rootless --load`; BuildKit Docker driver; no base/frontend pull; exit 0. |
| Direct image labels/architecture | PASS | Linux AMD64; project and RUN-0013 labels exact; no exposed port. |
| Marker extraction | PASS | Stopped container only; `docker cp`; byte comparison and SHA-256 match. |
| Compose scratch build | PASS with documented selector correction | Compose used the already-selected rootless context builder implicitly after explicit context-derived builder references were rejected before build. |
| Compose image labels/architecture | PASS | Linux AMD64; labels exact; no exposed port. |
| No service started | PASS | Direct container was created but never started; Compose created no container and `compose up` was not run. |
| No port published | PASS | Image exposed-port fields absent; direct port bindings empty; no Compose port definition/resource. |
| No image pushed or registry authentication | PASS | Builds used local `--load`/Compose export only; no push/auth command. |
| No privileged or multi-platform helper | PASS | No privileged mode, QEMU, `binfmt_misc`, additional node, or helper installation. |
| Cleanup | PASS | Zero RUN-0013 containers, images, networks, volumes, or temporary files remain. |
| Residual BuildKit cache | PASS/EXPECTED | Seven reclaimable records, 390 bytes total; retained without broad pruning. |
| Unrelated Docker state preserved | PASS | No system/buildx prune command; only operation-owned container/images/temp files removed. |
| Documentation aligned | PASS pre-publication | ADR, tooling policy, execution record, README, and build sequence describe the corrected builder model and update policy. |
| No restricted asset or secret published | PASS pre-commit | Six staged Markdown objects reviewed; zero binary, temporary context, secret, private network detail, client asset, or restricted path. |
| Git commit and push | PASS | Commit `d35ecf50c80c5bdb01ffdc6c189ab27f7f62f030` was pushed normally as a fast-forward. |
| Final refs match | PASS at governance verification | After first push all three refs equaled `d35ecf50c80c5bdb01ffdc6c189ab27f7f62f030`; completed records require one validation-only follow-up commit. |
| Lifecycle remains S0 | PASS | No advancement authorized; all new current records state `S0 — Proposed`. |

## Initial-attempt validation classification

The initial installation attempt was correctly stopped and was not assigned PASS. Official provenance, binary verification, installation, and CLI discovery passed. The literal-`default` builder requirement was incorrect for this context layout. No build or repository publication occurred, the verified plugin was retained, temporary state was removed, and the corrective operation revalidated every retained property before building.

## Corrective Compose classification

The installed Compose/Buildx combination rejected environment-variable and explicit-flag references to the context-derived builder before any build began, even while the stored context was `rootless`. These failed attempts created no Docker resource and changed no context.

With `rootless*` already selected, Compose succeeded by using the selected context-derived builder implicitly. Evidence includes the unchanged active context, the selected/running rootless Docker-driver builder before and after, zero manual builder instances, BuildKit/Bake output, the image loaded into the rootless daemon, and unchanged daemon/storage invariants. This is a compatibility behavior to preserve in the tooling policy, not authorization to create another builder.

## Publication validation

- Starting commit: `311be3d0e5178c8560f7a23da61c0806bea0c59c`.
- Governance commit: `d35ecf50c80c5bdb01ffdc6c189ab27f7f62f030`.
- Normal first push: PASS; remote fast-forwarded to the governance commit.
- First post-push refs: local HEAD = local `origin/main` = remote main = `d35ecf50c80c5bdb01ffdc6c189ab27f7f62f030`.
- Starting-history ancestry: PASS.
- Post-first-push tree: clean on `main`; 86 tracked files.
- Post-first-push runtime: Buildx v0.35.0 discoverable; context `rootless`; `fuse-overlayfs`; zero RUN-0013 resource.

## Validation-only record publication

Completing RUN-0013 and this record changes them after the first push. One additional commit with message `Record Docker Buildx rootless validation` is required and authorized. Its object ID and final local/origin/remote equality must be captured after the commit and normal push in the final operation report; the commit cannot contain its own object ID.

## Conclusion

Plugin provenance, installation, rootless builder discovery, direct native scratch building, Compose native scratch building, marker integrity, containment, cleanup, governance integrity, and publication pass. Overall validation is **PASS**. No AzerothCore acquisition or build occurred. The lifecycle remains **S0 — Proposed**.
