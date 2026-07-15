# VAL-0011 — blAIne AzerothCore Repository Publication Validation

## Record control

- Date: 2026-07-14
- Subject: `RUN-0011`
- Remote: `git@github.com:blainesmith33/blaine-azerothcore.git`
- Branch: `main`
- Status: **INCOMPLETE — push and remote-ref verification have not occurred**
- Lifecycle: `S0 — Proposed`

This record must not be marked PASS until the commit is pushed normally, the remote ref matches local HEAD, the starting README commit remains an ancestor, and the final working tree is reviewed.

## Pre-publication assertions

| Assertion | Result | Evidence |
|---|---|---|
| Correct local root | PASS | Canonical path `/run/media/deck/blAIneXPLAT/azerothcore`; `/dev/sda1` exFAT, read/write. |
| Correct remote | PASS | Origin is exactly `git@github.com:blainesmith33/blaine-azerothcore.git`; read-only `ls-remote` succeeded. |
| Existing remote history attached | PASS pre-commit | Local main and origin/main both start at `950f6bf09b7048aca7208ed9e2ae974290cdb16f`; no unrelated-history merge or replacement. |
| Root README preserved | PASS | Root hash matches the commit-pinned remote README; differing local README preserved under `records/source-state/`. |
| Restricted client payloads excluded | PASS for rule/preflight | Client roots, executables, MPQ/WTF, and client-root Data/Cache paths are ignored; physical preflight found none. |
| Raw documentation corpus excluded by default | PASS for rule/preflight | Raw corpus wildcard is ignored with only README/manifests/indexes exceptions; physical preflight found no corpus file. |
| Credentials/private keys excluded | PASS for rule/preflight | Credential/key/environment rules present; path and text scans found no candidate. |
| Database state/backups excluded | PASS for rule/preflight | Runtime/backups and compressed dumps ignored; legitimate uncompressed migration `*.sql` is not blanket-ignored. |
| Build/runtime outputs excluded | PASS for rule/preflight | Build, dependency, runtime, log, container, cache, and temporary patterns present; physical preflight found none. |
| No staged file over 25 MiB | PASS | Maximum staged blob is 28,572 bytes; zero exceeds 25 MiB. |
| No staged external-pointing symlink | PASS | All 77 staged additions are regular mode 100644 text; zero symlink mode. |
| No suspected secret in staged text | PASS | Staged binary-excluding scans returned zero paths for all secret categories; no value printed. |
| No Blizzard/WoW client asset staged | PASS | Restricted path/extension and binary-signature reviews returned zero staged candidate. |
| Commit contents reviewed | PASS pre-commit | 77 additions, zero modifications/deletions; all paths/modes/sizes/MIME types reviewed. The 169 `diff --check` diagnostics are inherited Markdown formatting retained intentionally. |
| Push succeeded | PENDING | No push yet. |
| Remote main matches local commit | PENDING | No publication commit yet. |
| Final working tree status | PENDING | Final commit/push not yet performed. |
| Lifecycle remains S0 | PASS | README/GOVERNANCE remain `S0 — Proposed`; no advancement authorized. |

## Required final validation

Before PASS, independently review cached status/stat/name-status/check output, every staged path and blob, ignored local content, commit tree, normal push result, local/origin/remote ref equality, starting-commit ancestry, tracked restricted-pattern absence, and final status. Record the real commit hash or hashes and any warning without exposing secret values.
