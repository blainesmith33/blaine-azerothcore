# VAL-0011 — blAIne AzerothCore Repository Publication Validation

## Record control

- Date: 2026-07-14
- Subject: `RUN-0011`
- Remote: `git@github.com:blainesmith33/blaine-azerothcore.git`
- Branch: `main`
- Status: **PASS — scaffold publication pushed and remote-ref verified**
- Lifecycle: `S0 — Proposed`

PASS was assigned only after the scaffold commit was pushed normally, the remote ref matched local HEAD, the starting README commit remained an ancestor, and the post-push working tree and commit tree were reviewed.

## Pre-publication assertions

| Assertion | Result | Evidence |
|---|---|---|
| Correct local root | PASS | Canonical path `/run/media/deck/blAIneXPLAT/azerothcore`; `/dev/sda1` exFAT, read/write. |
| Correct remote | PASS | Origin is exactly `git@github.com:blainesmith33/blaine-azerothcore.git`; read-only `ls-remote` succeeded. |
| Existing remote history preserved | PASS | Published commit `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a` has original README commit `950f6bf09b7048aca7208ed9e2ae974290cdb16f` as its direct parent. |
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
| Push succeeded | PASS | Normal `git push -u origin main` exited 0 and fast-forwarded `950f6bf..eabf64e`; no force push. |
| Remote main matches new local commit | PASS | Immediately after first push, HEAD, origin/main, and `git ls-remote` were all `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a`. |
| Final working tree status | PASS at scaffold verification | Clean after first push; zero ignored physical paths and zero untracked files. This completed record requires one validation-only follow-up commit. |
| Lifecycle remains S0 | PASS | README/GOVERNANCE remain `S0 — Proposed`; no advancement authorized. |

## Commit and remote verification

- Original remote README commit: `950f6bf09b7048aca7208ed9e2ae974290cdb16f`.
- Published governed scaffold commit: `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a`.
- Commit tree: 78 files, all mode 100644; maximum staged addition 28,572 bytes.
- First post-push refs: local HEAD = local origin/main = remote main = `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a`.
- `git log --oneline --decorate -5` showed the scaffold commit followed by the original `Create README.md` commit.
- Root README committed hash: `e61ac8b331463cd13a72de8f6e2178313529e72ce6cacd4ad4530673f7e3d6e7`, matching the starting remote README.
- Committed restricted-path, restricted-extension, non-regular-mode, >25 MiB, secret-category, and binary-signature reviews passed.

## Validation-only record publication

Completing this record changes RUN-0011 and VAL-0011 after the first push. One additional commit with message `Record blAIne AzerothCore publication validation` is therefore required and authorized. Its final object ID and local/origin/remote ref equality must be captured after the commit and normal push in the operation's final report; the commit cannot contain its own object ID.

## Conclusion

The governed scaffold publication passed. Existing history and the remote README were preserved; restricted assets, credentials, client payloads, raw acquired corpora, database state, build/runtime output, and recovery material were not published. No lifecycle advancement occurred. The project remains **S0 — Proposed**.
