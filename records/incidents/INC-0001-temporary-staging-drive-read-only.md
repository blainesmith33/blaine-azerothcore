# INC-0001 — Temporary staging drive appeared read-only

## Record control

- Identifier: `INC-0001`
- Date: 2026-07-14
- Lifecycle status: `S0 — Proposed`
- Incident status: contained; diagnosis completed; human decision required
- Severity: blocking, with no known data loss
- Authoritative project root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Related records: `AUD-0002`, `RUN-0004`, `VAL-0004`

## Intended operation

The authorized operation was to create a temporary, migration-ready AzerothCore scaffolding workspace. It did not authorize software installation, source acquisition, runtime data, mount changes, filesystem repair, or lifecycle advancement.

The exact proposed staging path was:

`/run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/`

## Initial observation and abort

The initial restricted inspection namespace reported:

- backing partition: `/dev/sdb1`;
- filesystem: `ext4`;
- mountpoint: `/run/media/deck/blAIne-476GB-SAM`;
- observed options: `ro,nosuid,nodev,noatime,errors=remount-ro`;
- proposed staging root: absent.

The non-overwrite prechecks passed: the governance root resolved correctly, RUN-0001 through RUN-0003 and VAL-0001 through VAL-0003 were present, the staging parent existed, and the proposed staging root did not exist. Creation stopped before any staging-drive write was attempted. No staging directory or file was created.

## Diagnostic correction

AUD-0002 established that the initial `ro` observation was namespace-specific. A read-only host-namespace inspection at 2026-07-14T17:33:22-07:00 reported the same mount as:

`rw,nosuid,nodev,noatime,errors=remount-ro`

The host block device and partition both reported `ReadOnly: false`, and `blockdev --getro` returned `0` for both. The current attachment was logged as mounted read/write at 17:25:57. Therefore, the host filesystem was not read-only at the time of diagnosis; the attempted scaffold was blocked by the execution environment's restricted view and write boundary.

Historical current-boot logs also show earlier USB/UAS disconnect, device-offline, I/O, and journal failures involving this filesystem during previous attachment cycles. Those events are a real reliability concern, but they are not evidence that the current host mount is read-only.

## Impact

- Temporary scaffold creation was blocked and not retried.
- The authoritative governance project was not replaced.
- No known data loss resulted from the aborted attempt.
- The proposed staging workspace remains absent.

## Containment

- No write was attempted on the staging drive.
- No forced mount, remount, unmount, or filesystem repair was attempted.
- No permissions, ownership, services, packages, containers, databases, repositories, source, clients, or Git state were changed.
- The lifecycle remains `S0 — Proposed`.

## Required next decision

Human approval is required before a new scaffold-creation execution. Based on AUD-0002, the next execution should use a host-write authorization narrowly limited to the exact proposed staging root, reverify the host-namespace mount as read/write, reverify that the staging root is absent, and preserve the prior non-overwrite controls. No remount or repair is justified by the current read-only diagnosis alone.

