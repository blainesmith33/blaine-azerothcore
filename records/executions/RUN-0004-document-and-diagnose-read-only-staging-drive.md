# RUN-0004 — Document and diagnose read-only staging drive

## Record control

- Identifier: `RUN-0004`
- Start: 2026-07-14T17:32:00-07:00
- Completion: 2026-07-14T17:36:55-07:00
- Working directory: `/home/deck`
- Lifecycle status: `S0 — Proposed`
- Result: read-only diagnosis completed; staging creation not retried
- Related records: `INC-0001`, `AUD-0002`, `VAL-0004`

## Authorization boundary

Only read-only system diagnosis and creation of four Markdown records under `/run/media/deck/blAIneXPLAT/azerothcore/records/` were authorized. No mount, unmount, remount, repair, write test, staging retry, or system modification was authorized.

## Files created

```text
/run/media/deck/blAIneXPLAT/azerothcore/records/incidents/INC-0001-temporary-staging-drive-read-only.md
/run/media/deck/blAIneXPLAT/azerothcore/records/audits/AUD-0002-temporary-staging-drive-read-only-diagnosis.md
/run/media/deck/blAIneXPLAT/azerothcore/records/executions/RUN-0004-document-and-diagnose-read-only-staging-drive.md
/run/media/deck/blAIneXPLAT/azerothcore/records/validations/VAL-0004-read-only-staging-drive-diagnosis-validation.md
```

No file was created on `/dev/sdb1` or beneath the staging parent.

## Command ledger

Commands are listed in execution order. Wrapper functions only printed each command and its exit status; they did not alter command behavior.

### Precondition and non-overwrite gate

| Command | Exit | Observation |
|---|---:|---|
| `realpath /run/media/deck/blAIneXPLAT/azerothcore` | 0 | Resolved to the authoritative path. |
| `find GOVERNANCE_ROOT/records/executions -maxdepth 1 -type f -name 'RUN-0001-*.md' -print` | 0 | RUN-0001 present. |
| equivalent `find` for RUN-0002 and RUN-0003 | 0 each | Both present. |
| equivalent `find` for VAL-0001 through VAL-0003 | 0 each | All present. |
| `test -d /run/media/deck/blAIne-476GB-SAM/blAIne` | 0 | Parent present. |
| `test -e /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging` | 1 | Proposed staging root absent; expected. |
| `test -e` for each proposed INC-0001/AUD-0002/RUN-0004/VAL-0004 path | 1 each | All absent before creation; expected. |
| combined gate | 0 | Safe to create new records. |

### Time, mount, storage, and path inspection

| Command | Exit |
|---|---:|
| `date --iso-8601=seconds` | 0 |
| `pwd` | 0 |
| `findmnt -T /run/media/deck/blAIne-476GB-SAM/blAIne` | 0 |
| `findmnt -T /run/media/deck/blAIne-476GB-SAM/blAIne -o SOURCE,TARGET,FSTYPE,OPTIONS` | 0 |
| `lsblk -b -p -o NAME,PATH,PKNAME,TYPE,SIZE,VENDOR,MODEL,SERIAL,TRAN,RO,FSTYPE,LABEL,UUID,MOUNTPOINTS` | 0 |
| `df -B1 -T /run/media/deck/blAIne-476GB-SAM/blAIne` | 0 |
| `stat -c 'path=%n owner=%U(%u) group=%G(%g) mode=%A(%a) size=%s' /run/media/deck/blAIne-476GB-SAM/blAIne` | 0 |
| `namei -l /run/media/deck/blAIne-476GB-SAM/blAIne` | 0 |
| `findmnt -T /home/deck -o SOURCE,TARGET,FSTYPE,OPTIONS` | 0 |
| `df -B1 -T /home/deck` | 0 |

### Device and mount namespace inspection

| Command | Restricted exit | Host-namespace exit | Notes |
|---|---:|---:|---|
| `cat /sys/block/sdb/ro` | 0 | not repeated | Output `0`. |
| `cat /sys/block/sdb/device/vendor` | 0 | not repeated | Samsung. |
| `cat /sys/block/sdb/device/model` | 0 | not repeated | SSD 830 Series. |
| `cat /sys/block/sdb/device/state` | 0 | not repeated | `running`. |
| `blockdev --getro /dev/sdb` | 1 | 0 | Device node hidden in restricted namespace; host output `0`. |
| `blockdev --getro /dev/sdb1` | 1 | 0 | Device node hidden in restricted namespace; host output `0`. |
| `udisksctl info -b /dev/sdb` | 1 | 0 | D-Bus blocked in restricted namespace; host reported `ReadOnly: false`. |
| `udisksctl info -b /dev/sdb1` | 1 | 0 | Same limitation and result. |
| `udevadm info --query=all --name=/dev/sdb` | 1 | 0 | Device hidden in restricted namespace; host data collected. |
| `udevadm info --query=all --name=/dev/sdb1` | 1 | 0 | Same limitation. |
| `rg -n 'sdb\|blAIne-476GB-SAM' /proc/self/mountinfo` | 0 | 0 | Restricted namespace `ro`; host namespace `rw`. |
| `rg -n 'sdb\|blAIne-476GB-SAM' /proc/mounts` | 0 | 0 | Restricted namespace `ro`; host namespace `rw`. |
| `rg -n -i 'sdb\|70913570\|blAIne-476GB-SAM' /etc/fstab` | 1 | not repeated | No match. |
| host `findmnt -T ... -o SOURCE,TARGET,FSTYPE,OPTIONS` | not applicable | 0 | Host mount `rw`. |

### Kernel, journal, and health inspection

| Command | Exit | Notes |
|---|---:|---|
| `command -v smartctl` | 0 | `/usr/bin/smartctl`. |
| `smartctl -i -H -A /dev/sdb` | 2 | Identity access not permitted; no SMART claim made. |
| `journalctl -k -b --no-pager \| rg -n -i RELEVANT_PATTERN` | 0 | Relevant current-boot events collected; output was large and is interpreted in AUD-0002. |
| `dmesg \| rg -n -i RELEVANT_PATTERN` | 1 | Kernel buffer access denied. |
| `journalctl -k -b --since '2026-07-14 17:24:00' --no-pager` | 0 | Current attachment timeline collected. |
| `journalctl -b --since '2026-07-14 17:24:00' --until '2026-07-14 17:40:00' --no-pager \| rg -n -i FILTER` | 0 | UDisks, recovery, and mount events collected. |
| final host `date --iso-8601=seconds` | 0 | `2026-07-14T17:33:22-07:00`. |

The relevant pattern was:

```text
sdb|sdb1|EXT4-fs|filesystem error|remount-ro|I/O error|Buffer I/O|blk_update_request|write protect|read-only|USB reset|uas|usb-storage|device offline
```

### Record creation and validation commands

| Command | Exit | Notes |
|---|---:|---|
| governed patch creating the four new Markdown records | 0 | Paths limited to the authoritative governance root. |
| explicit validation command recorded in VAL-0004 | 0 | All validation assertions passed. |
| complete record-print command | 0 | Read-only output of all four records. |

System inspection completed at 2026-07-14T17:33:22-07:00. Record creation and explicit validation were completed by the execution completion time above.

## Warnings and limitations

- The restricted execution namespace intentionally presented paths outside its writable roots as read-only. This caused the original staging blocker and prevented direct device-node/D-Bus inspection.
- Host-namespace authorization was used only for read-only queries. It revealed the actual host mount as read/write.
- Historical logs show serious intermittent USB/UAS I/O failures. Exact hardware localization was not possible without SMART access or physical testing.
- Device names changed during earlier disconnect/reconnect events. AUD-0002 uses UUID and identity evidence to avoid treating every historical `/dev/sdb` line as the same attachment.
- `dmesg` and SMART access failures are preserved rather than omitted.

## Non-modification attestation

This execution created only the four listed Markdown records under the authoritative governance root. It did not modify `/dev/sdb`, `/dev/sdb1`, their filesystem, mount state, mount configuration, permissions, or ownership. It did not perform mount, unmount, remount, fsck, e2fsck, repair, package, service, container, database, repository, source, game-client, firewall, Git, or scaffold operations.

The project remains `S0 — Proposed`.
