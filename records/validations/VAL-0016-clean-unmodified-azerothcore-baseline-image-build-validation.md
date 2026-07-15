# VAL-0016 — Clean Unmodified AzerothCore Baseline Image Build Validation

## Record control

- Date: 2026-07-15
- Subject: `BLD-ACORE-WOTLK-001`, `RUN-0016`
- Branch: `main`
- Status: **PASS — technical build, inspection, containment, evidence, staged audit, and first governance publication verified**
- Lifecycle: `S0 — Proposed`

PASS was assigned only after the four target builds, binary/package inspection, containment checks, evidence sealing, intended staged-content review, normal first commit/push, and matching post-push local/origin/remote refs succeeded.

## Validation matrix

| Assertion | Result | Evidence |
|---|---|---|
| Governance synchronized start | PASS | Clean main; no active operation; starting HEAD/origin/remote `43c22db051fd2fa2117e614fd29025b4eff92ed8`. |
| Exact approved checkout | PASS | `CHK-ACORE-WOTLK-001`, official origin, detached and clean. |
| Source pin/tree/archive | PASS | Exact governed SHA, tree, and `26375e...d2556b1`. |
| Source Git integrity | PASS | Strict fsck; self-contained Git; no alternates/hardlinks/partial dependency. |
| Symlink and executable modes | PASS | Exact symlink; 42 executable entries. |
| Dockerfile identity | PASS | Governed path and `5a43e9...c7b6e`. |
| No custom module | PASS | Pinned module scaffolding only; no blAIne module/change. |
| Host resource gate | PASS | More than 8 GiB available RAM+free swap; about 1.1 TiB free internal storage. |
| Rootless Docker unchanged | PASS | Engine 29.6.1; rootless security/context/socket; configuration hashes unchanged. |
| Buildx version/builder | PASS | v0.35.0, existing `rootless` Docker-driver builder, BuildKit v0.31.1; no extra builder. |
| Unused `default` builder | PASS classification | Still maps to absent rootful socket; not selected, changed, or treated as a rootless-builder failure. |
| Storage driver | PASS | `fuse-overlayfs`; internal Docker root unchanged. |
| Official Ubuntu identity | PASS | `docker.io/library/ubuntu:24.04`. |
| Ubuntu digest pin | PASS | Index `4fbb8e...2d90`; AMD64 child `52df9b...3faf`. |
| Worldserver build | PASS | Exit 0; direct target, compiled/installed/exported/loaded. |
| Authserver build | PASS | Exit 0; cached shared stage, final export/load. |
| DB-import build | PASS | Exit 0; compiled tool and source SQL, not run. |
| Tools build | PASS | Exit 0; four expected extractors, not run. |
| Image labels | PASS | Exact source/build/source-ID/checkout-ID/clean-baseline labels on four images. |
| Platform | PASS | All four Linux AMD64. |
| Users/commands | PASS | All non-root `acore`; correct auth/world/db commands; tools intentionally no command. |
| Expected binaries | PASS | Seven regular executable paths exist with mode 0755 and `acore:acore`. |
| Binary hashes / ELF | PASS | Seven SHA-256 values recorded; ELF64 x86-64 PIE, nonzero size. |
| Library dependencies | PASS | `ldd` found zero unresolved libraries; binaries not executed. |
| Package manifests | PASS | 114 package/version rows per image; SHA-256 `dd4bba...02de5`. |
| Configuration templates | PASS | Three `.conf.dist` templates; no active config generated. |
| Client-data target/downloader | PASS containment | Not built or executed; zero client-data files. |
| ChromieCraft/client assets | PASS containment | None acquired or generated. |
| MySQL/database | PASS containment | No MySQL image/container/volume/connection/import; source SQL only. |
| Service/server execution | PASS containment | Exact process audit zero; no server/importer/extractor invocation. |
| Port/listener | PASS containment | No image port; pre/post host listeners identical; no publication. |
| Compose | PASS containment | No build/up/run invocation. |
| Registry auth/push | PASS containment | Anonymous base resolution only; no push/repo digest. |
| Privileged/helper changes | PASS containment | No privileged mode, QEMU, binfmt, host network, context/config change. |
| Inspection cleanup | PASS | Zero containers and volumes; temporary copied binaries removed. |
| Final image set | PASS | Exactly four approved local tags; no client/MySQL/dev image. |
| Networks/volumes | PASS | Only default networks; zero volumes. |
| BuildKit cache | PASS | Expected 7.431 GB retained; no prune. |
| Source post-build | PASS | Detached, clean; pin/tree/archive digest unchanged. |
| Evidence integrity | PASS | 71 files, 70 verified checksum entries; manifest SHA `ffac7c...8cb25`. |
| Evidence privacy/restrictions | PASS | Zero secret/private-network/binary/restricted payload indicators after type-only sanitization. |
| Documentation accuracy | PASS pre-publication | ADR, guide, manifest, RUN, VAL, README, and sequence align with evidence. |
| Historical integrity | PASS pre-staging | No historical RUN/VAL modification intended. |
| Staged-content audit | PASS | Exactly eight intended text paths; whitespace/full-diff/size/type/secret/private-network/restricted-content checks passed. |
| Governance commit/push | PASS | `a3d8ec9b1321a0b7017972d1ec98d416ef1582af` pushed normally as a fast-forward; no force. |
| First post-push refs | PASS | Local HEAD, origin/main, and remote main all equaled the governance commit. |
| Lifecycle | PASS | Remains S0 — Proposed. |

## Technical conclusion

The four approved clean local images were built from the exact clean checkout using the pinned Ubuntu image digest and existing rootless builder. Compiled server/import/extractor outputs passed non-executing file, hash, ELF, package, and dependency inspection. Client-data, database, service, port, registry, and source-change containment passed.

The build is identity-pinned but not fully hermetic because Ubuntu packages were acquired from mutable official repositories. Actual database operation, server startup, gameplay behavior, client compatibility, extractor correctness, and public networking remain unvalidated.

## Publication boundary

The first publication staged exactly eight intended text/current-document paths, created commit `a3d8ec9b1321a0b7017972d1ec98d416ef1582af`, and pushed it normally. All three refs matched after that push, and starting history remained an ancestor.

Completing RUN-0016 and this record requires the one authorized validation-only follow-up commit. Its object ID cannot be included in its own content; final local/origin/remote equality is captured after that normal push in the operation's final verification and report.

Lifecycle remains **S0 — Proposed**.
