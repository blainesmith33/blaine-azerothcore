# RUN-0006 — Formalize Realm Control and Feature Package Scaffold

## Record control

- Identifier: RUN-0006
- Start: 2026-07-14T18:12:22-07:00
- Completion: 2026-07-14T18:35:45-07:00
- Working directory: /home/deck
- Lifecycle: S0 — Proposed
- Result: completed and validated
- Related decisions: ADR-0007, ADR-0008, ADR-0009
- Related validation: VAL-0006

## Authorized roots

- Governance: /run/media/deck/blAIneXPLAT/azerothcore
- Temporary staging: /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging

No write was authorized or performed outside these roots.

## Safety-gate evidence

- Both roots resolved exactly to their expected canonical paths.
- The staging root existed.
- The existing 84-entry staging checksum passed before changes.
- Host findmnt reported /dev/sdb1, ext4, rw,nosuid,nodev,noatime,errors=remount-ro.
- ADR-0005, ADR-0006, RUN-0005, and VAL-0005 existed.
- Governance still stated S0 — Proposed.
- No proposed new path existed.
- The kernel journal from RUN-0005 completion through the gate contained zero matching ext4, journal, I/O, USB, UAS, remount, or reset errors.
- Before-state hashes were captured for every subsequently amended file.

## Governance files created

- records/decisions/ADR-0007-blaine-realm-control-architecture.md
- records/decisions/ADR-0008-full-stack-feature-package-architecture.md
- records/decisions/ADR-0009-azerothcore-project-service-and-hosting-boundary.md
- records/executions/RUN-0006-formalize-realm-control-and-feature-package-scaffold.md
- records/validations/VAL-0006-realm-control-and-feature-package-scaffold-validation.md
- docs/architecture/blaine-realm-control-architecture.md
- docs/architecture/command-broker-and-execution-model.md
- docs/architecture/full-stack-feature-package-architecture.md
- docs/architecture/client-addon-server-module-contract.md
- docs/requirements/realm-control-requirements.md
- docs/requirements/feature-package-requirements.md
- docs/requirements/command-definition-requirements.md
- docs/requirements/addon-acquisition-and-inventory-requirements.md
- docs/requirements/azerothcore-project-service-boundary.md
- docs/interfaces/README.md
- docs/interfaces/command-contexts.md
- docs/interfaces/operation-risk-model.md
- docs/interfaces/protocol-versioning.md
- docs/presets/README.md
- docs/presets/preset-lifecycle.md
- docs/presets/dungeon-party-preparation-concept.md

## Staging files created

- control-plane/README.md
- control-plane/frontend/README.md
- control-plane/backend/README.md
- control-plane/backend/api/README.md
- control-plane/backend/command-broker/README.md
- control-plane/backend/authorization/README.md
- control-plane/backend/services/README.md
- control-plane/backend/adapters/README.md
- control-plane/backend/adapters/azerothcore/README.md
- control-plane/backend/adapters/container-runtime/README.md
- control-plane/backend/adapters/database/README.md
- control-plane/backend/adapters/host/README.md
- control-plane/backend/adapters/filesystem/README.md
- control-plane/backend/adapters/metrics/README.md
- control-plane/command-catalog/README.md
- control-plane/command-catalog/templates/command-definition-template.yaml
- control-plane/command-catalog/examples/character-level-concept.yaml
- control-plane/command-catalog/examples/realm-status-concept.yaml
- control-plane/presets/README.md
- control-plane/presets/templates/preset-definition-template.yaml
- control-plane/presets/examples/level-15-dungeon-party-concept.yaml
- control-plane/schemas/README.md
- control-plane/schemas/operation-request.schema.yaml
- control-plane/schemas/operation-result.schema.yaml
- control-plane/schemas/command-definition.schema.yaml
- control-plane/configuration/README.md
- control-plane/configuration/control-plane.env.example
- control-plane/migrations/README.md
- control-plane/tests/README.md
- control-plane/documentation/README.md
- control-plane/records/README.md
- addons/README.md
- addons/existing-inventory/README.md
- addons/candidates/README.md
- addons/upstream-reference/README.md
- addons/blaine/README.md
- addons/manifests/README.md
- addons/manifests/addon-registry.yaml
- addons/manifests/addon-manifest-template.yaml
- addons/profiles/README.md
- addons/patches/README.md
- features/README.md
- features/registry/README.md
- features/registry/feature-registry.yaml
- features/templates/full-stack-feature/README.md
- features/templates/full-stack-feature/FEATURE-MANIFEST.yaml
- features/templates/full-stack-feature/server-module/README.md
- features/templates/full-stack-feature/client-addon/README.md
- features/templates/full-stack-feature/realm-control/README.md
- features/templates/full-stack-feature/protocol/README.md
- features/templates/full-stack-feature/presets/README.md
- features/templates/full-stack-feature/database/README.md
- features/templates/full-stack-feature/configuration/README.md
- features/templates/full-stack-feature/documentation/README.md
- features/templates/full-stack-feature/tests/README.md
- features/candidates/README.md
- features/upstream-reference/README.md
- features/blaine/README.md
- features/experiments/README.md
- protocols/README.md
- protocols/schemas/README.md
- protocols/compatibility/README.md

## Files narrowly amended and hashes

| File | Before SHA-256 | After SHA-256 |
|---|---|---|
| README.md | 8cdb23890f24452d1ea09f5388cabc5208765103efeca3a5c913e2b60391040b | 1ea5f5c7cdf3b1aaa0806781c16cd16e185ab9c373ffd48c109fdf9e97a45a66 |
| GOVERNANCE.md | 95bdd3ad7cc21f144f94eae1587ed98742962f3b01d52cbb56e53d393e2e4b1b | b76117cb97cc1d8c039f2ae4deda242cd7f1baa6d291a95b09c7cda54a909fe0 |
| docs/governance/project-charter.md | 5b34a942a908acb8188323e1367610073c4ec3b58998fa41489792483bc46d68 | d71883abd57f51667bb159acd013bf0a76ae33621154c2b8b1981ece135936e7 |
| docs/architecture/multi-realm-architecture.md | 82c99d1f3a23f9515f2d9e1553b58a03ab5560654558089f8cf858a3b69d63d8 | cf52a328c4142fbb0a37cdefc6505b48d1ea3a7b1e8aa005ff5c3c8d38fa63b1 |
| docs/architecture/module-suite-architecture.md | c1e4c16f8a13cfc92a4c94c3bab3698810a011a9bfb505722c386cb3e89987f7 | ed1bda96251ade2564582bff9857b7e247768ee468640ddfe1b995d61676e51b |
| docs/governance/module-version-and-integration-policy.md | 921ca2562d7b2b4a7e8de811c8315c4efb47b37625f052edfc0a6e2ec823584c | 5d8ee330645659aac1e840fa8301562dbc3e2be00a3c2b5eae81c34f880dbcc6 |
| docs/governance/module-provenance-and-license-policy.md | 1a2bcca61c47d1a0ab56c7e0d2d6fe615164f94a16cde1af4b746d52271fb302 | 788884de836ac5a2568bf59ac531bf771776342208120128aeba0d3cce63804a |
| docs/requirements/multi-realm-and-module-integration-requirements.md | 4787213208e7ffe754c050129715648acb00592dcf283b429d2fdf55097d6ce3 | a8168c91ffa588af73ecb36a7374c5c97a7a0aa2b3a7f64d657b821bc1588a10 |
| docs/instances/instance-classification.md | 9d9ac63f04c8b3eaf116b5bc91eb297380025fffd93144d6077e328f49676d0b | 89b91679fe9c51ee08f435c5d2341b55a68a867778466a46a89138b8f5a6ba61 |
| STAGING/README.md | 858fbac9cdef670437d23fec1bb15999a3faa683fe233edb0dc210dc6035b570 | f5679a5924a467afb6246a7ea55ada440d2e028ab596832851823452d8ac5168 |
| STAGING/STAGING-MANIFEST.md | 205a2f9f1c3ce3c27c16cd98df73535bc935e3370af9cd4bc77644aa8ef13585 | 6fba292f9c824260d63baaf081051669f8951db1949a8499f6cbd3bf3aded688 |
| STAGING/manifests/README.md | a2ceaae70ef75afad0674e4cf5ff0eb003556f137edadf1588809077fc28b5d3 | 6c682697683ef292f4fa1c79d59ec1c2a3b6c71ca417b6f91251c3d995cc4a7c |
| STAGING/modules/README.md | 07995a1512e5898083e017e262988ff8e6854c1a9eb5ac6a970a72759df085d6 | a35b854b93c5057a83f04b4ad176e2ec85c7dcbf0051dfa8b63f202bcf90a4c1 |
| STAGING/records/validations/VAL-0005-created-files.sha256 | 7a1e745b43d75dde644e2970f9c550bd34dd0e65b372802f02e53b01c266083b | e2d8ac105a8b891be6f68dcf07c45fcdb1360102f02c949a43daa61e38d63570 |

The existing module registry was not amended. Its before and after SHA-256 remained `72988e5319772cc73fb31b8422290350c139109f278dc528d5d3b79d05ba3be9`.

## Command and operation ledger

| Command or operation | Exit | Observation |
|---|---:|---|
| `pwd`; `realpath` for both authorized roots; prerequisite `test` checks | 0 | Canonical roots and prerequisites passed. |
| `sha256sum -c records/validations/VAL-0005-created-files.sha256` before writes | 0 | Existing 84 entries passed. |
| `findmnt -T STAGING -o SOURCE,TARGET,FSTYPE,OPTIONS` | 0 | /dev/sdb1, ext4, rw. |
| `journalctl -k --since RUN-0005_COMPLETION --grep RELEVANT_STORAGE_PATTERN` | 1 | journalctl reports no match with exit 1; zero relevant lines. |
| `sha256sum` over planned amendment files and module registry | 0 | Before-state evidence captured. |
| `sed`, `tail`, and bounded `find` inspections of governing and staging documents | 0 | Existing meanings and insertion contexts reviewed. |
| First governance `apply_patch` batch | 1 | Twelve intended files were created, then the batch stopped because docs/interfaces did not yet exist. No unintended path was written. |
| `mkdir -p GOV/docs/interfaces GOV/docs/presets` | 0 | Missing authorized parents created. |
| Corrective governance `apply_patch` batch | 0 | Remaining seven pre-record documents created without overwrite. |
| `mkdir -p` for the exact control-plane, addons, features, and protocols staging directories | 0 | Authorized directory tree created. |
| Staging `apply_patch` batch | 0 | Sixty-two placeholder and schema files created. |
| Narrow-amendment `apply_patch` batch | 0 | Thirteen existing documentation files amended. |
| `sha256sum` over amended files and module registry | 0 | After-state hashes captured; module registry unchanged. |
| `python3 -c` PyYAML safe-load, tabs, empty/null validator | 0 | 20 YAML files parsed; zero failures, tabs, or empty/null values. |
| `find scripts -name '*.sh' -exec bash -n ...` | 0 | Four scripts passed Bash syntax. |
| `bash SCRIPT --help` for each existing script | 0 | Four help paths passed. |
| `bash scripts/validate-staging-layout.sh --staging-root STAGING` | 0 | Existing staging validation passed. |
| First generated `python3 -c` required-path validator | 1 | Shell quoting rendered embedded newlines literally; Python raised SyntaxError. Read-only; no state changed. |
| Corrected quoted `python3 -c` required-path validator | 0 | 19 pre-record governance files and 62 staging files present; no escapes, unexpected files, or README boundary failures. |
| `python3 -c` registry, marker, content, provenance, lifecycle, and prohibition validator | 0 | 19 checks passed; zero failures. |
| `find . -type f ! -path CHECKSUM -print0 | sort -z | xargs -0 sha256sum > CHECKSUM` | 0 | Manifest regenerated with 146 entries. |
| `sha256sum -c CHECKSUM` before sync | 0 | All 146 entries passed. |
| `sync -f /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging` | 0 | Filesystem-scoped flush completed. |
| `sha256sum -c --quiet CHECKSUM` after sync | 0 | All 146 entries passed. |
| `date`; `findmnt`; `stat`; `df -B1 -T`; `wc -l CHECKSUM`; `sha256sum CHECKSUM` | 0 | Mount, capacity, owner, entry count, and checksum captured. |
| Final `journalctl -k --since 2026-07-14T18:03:47-07:00 --until 2026-07-14T18:28:37-07:00 --grep RELEVANT_STORAGE_PATTERN` | 1 | `-- No entries --`; zero matching kernel errors. |
| First final semantic validator | 1 | Twenty-four checks passed; ADR-0009 used one coordinated negative sentence where the validator required three explicit prohibitions. |
| Clarity-only ADR-0009 `apply_patch` | 0 | Split the hosting, access-sale, and proprietary-content-sale prohibitions into explicit sentences without changing the decision. |
| Corrected final explicit path, content, lifecycle, and record validation | 0 | Eighteen consolidated checks passed; zero failures. |
| Final `date` and `findmnt -T STAGING` | 0 | At 18:32:48, /dev/sdb1 remained ext4/rw. |
| Final `journalctl` from 18:28:37 through 18:32:48 | 1 | `-- No entries --`; zero new matching kernel errors. |
| First closing consistency command | 1 | Invoked the relative-path checksum manifest from /home/deck; 146 files were consequently reported unreadable. This was a read-only working-directory error, not a checksum mismatch. |
| Corrected closing consistency command from the staging root | 0 | Records, module hash, layout, Bash, checksum, Git, lifecycle, and count checks passed. |
| Non-host final display command | 0 | The restricted execution namespace reported the known synthetic ro view described by AUD-0002; this was not treated as host state. |
| Authoritative host `date` and `findmnt -T STAGING` reconciliation | 0 | At 18:35:45, host /dev/sdb1 remained ext4/rw. |
| Authoritative host `journalctl` from 18:32:48 through 18:35:45 | 1 | `-- No entries --`; zero new matching kernel errors. |

The four exit-1 events above are preserved. None caused an unauthorized write: the first was an intended patch stopped at a missing parent after creating only authorized new files; the second was a read-only quoting error corrected transparently; the third identified wording that was made explicitly testable without changing the approved boundary; the fourth invoked a valid relative-path checksum manifest from the wrong working directory.

## Validation observations

- PyYAML was available and safe-loaded all 20 staging YAML files.
- YAML tab count: 0.
- YAML empty/null value count: 0.
- Addon registry entries: 0.
- Feature registry entries: 0.
- Module registry hash change: none.
- Script syntax/help failures: 0.
- Checksum entries: 146.
- Pre-sync checksum failures: 0.
- Post-sync checksum failures: 0.
- Post-write mount: /dev/sdb1 ext4 rw,nosuid,nodev,noatime,errors=remount-ro.
- Available staging bytes: 477177643008.
- Post-write relevant kernel-error count: 0.

## Prohibited operations and current status

No software, package, AzerothCore source, module, addon, client, repository, database, container, image, service, firewall, network, mount, repair, Git, credential, secret, game command, SQL operation, or runtime implementation was acquired, created, started, changed, or validated. The user's separate addon folder was not located, scanned, copied, renamed, hashed, or modified.

blAIne remains the broader umbrella identity. The AzerothCore project and blAIne Realm Control remain subordinate. Final lifecycle status is S0 — Proposed.
