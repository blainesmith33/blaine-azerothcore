# AUD-0002 — Temporary staging drive read-only diagnosis

## Record control

- Identifier: `AUD-0002`
- Evidence date: 2026-07-14
- Evidence interval: 2026-07-14T17:32:00-07:00 through 2026-07-14T17:33:22-07:00
- Working directory: `/home/deck`
- Lifecycle status: `S0 — Proposed`
- Scope: read-only diagnosis of `/dev/sdb1` and `/run/media/deck/blAIne-476GB-SAM/blAIne`
- Related records: `INC-0001`, `RUN-0004`, `VAL-0004`

## Scope and method

This audit distinguishes raw observation from interpretation. It performed no mount operation, filesystem repair, write test, benchmark, package or service action, repository action, or staging retry. Device names in historical kernel logs are treated as dynamic; filesystem UUID `70913570-fb08-4b22-a534-a8e658cb86d4` and hardware identity were used where needed to avoid conflation.

## Preconditions

### Commands

```text
realpath /run/media/deck/blAIneXPLAT/azerothcore
find .../records/executions -maxdepth 1 -type f -name 'RUN-0001-*.md' -print
find .../records/executions -maxdepth 1 -type f -name 'RUN-0002-*.md' -print
find .../records/executions -maxdepth 1 -type f -name 'RUN-0003-*.md' -print
find .../records/validations -maxdepth 1 -type f -name 'VAL-0001-*.md' -print
find .../records/validations -maxdepth 1 -type f -name 'VAL-0002-*.md' -print
find .../records/validations -maxdepth 1 -type f -name 'VAL-0003-*.md' -print
test -d /run/media/deck/blAIne-476GB-SAM/blAIne
test -e /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging
test -e RECORD_PATH
```

### Raw observations

- Governance realpath: `/run/media/deck/blAIneXPLAT/azerothcore`
- Required RUN and VAL records 0001–0003: present
- Staging parent: present
- Proposed staging root: absent
- INC-0001, AUD-0002, RUN-0004, and VAL-0004 before creation: absent
- Gate exit status: `0`

### Interpretation

The non-overwrite and authority preconditions passed.

## Mount and filesystem observations

### Restricted inspection namespace

Command:

```text
findmnt -T /run/media/deck/blAIne-476GB-SAM/blAIne -o SOURCE,TARGET,FSTYPE,OPTIONS
```

Raw observation, exit `0`:

```text
SOURCE    TARGET                           FSTYPE OPTIONS
/dev/sdb1 /run/media/deck/blAIne-476GB-SAM ext4   ro,nosuid,nodev,noatime,errors=remount-ro
```

Commands and raw observations, exit `0`:

```text
rg -n 'sdb|blAIne-476GB-SAM' /proc/self/mountinfo
35:1379 1373 8:17 / /run/media/deck/blAIne-476GB-SAM ro,nosuid,nodev,noatime master:507 - ext4 /dev/sdb1 rw,errors=remount-ro

rg -n 'sdb|blAIne-476GB-SAM' /proc/mounts
35:/dev/sdb1 /run/media/deck/blAIne-476GB-SAM ext4 ro,nosuid,nodev,noatime,errors=remount-ro 0 0
```

### Host namespace

The same read-only queries were repeated with host-namespace authorization. No write or mount action was requested or performed.

Raw observation at 2026-07-14T17:33:22-07:00, exit `0`:

```text
SOURCE    TARGET                           FSTYPE OPTIONS
/dev/sdb1 /run/media/deck/blAIne-476GB-SAM ext4   rw,nosuid,nodev,noatime,errors=remount-ro
```

Raw host mountinfo, exit `0`:

```text
45:538 27 8:17 / /run/media/deck/blAIne-476GB-SAM rw,nosuid,nodev,noatime shared:507 - ext4 /dev/sdb1 rw,errors=remount-ro
```

Raw host `/proc/mounts`, exit `0`:

```text
/dev/sdb1 /run/media/deck/blAIne-476GB-SAM ext4 rw,nosuid,nodev,noatime,errors=remount-ro 0 0
```

### Capacity and path metadata

Commands:

```text
df -B1 -T /run/media/deck/blAIne-476GB-SAM/blAIne
stat -c 'path=%n owner=%U(%u) group=%G(%g) mode=%A(%a) size=%s' /run/media/deck/blAIne-476GB-SAM/blAIne
namei -l /run/media/deck/blAIne-476GB-SAM/blAIne
```

Raw observations, all exit `0`:

```text
Filesystem     Type    1B-blocks      Used    Available Use% Mounted on
/dev/sdb1      ext4 502921060352 120090624 477178753024   1% /run/media/deck/blAIne-476GB-SAM

path=/run/media/deck/blAIne-476GB-SAM/blAIne owner=deck(1000) group=deck(1000) mode=drwxr-xr-x(755) size=4096
```

`namei` showed the final `blAIne` directory as `deck:deck` mode `0755`. The mountpoint itself appeared mode `0777` inside the restricted namespace.

### Interpretation

The original `ro` result was imposed by the restricted inspection namespace. It did not describe the host mount. The host mount was read/write throughout the diagnostic evidence interval. The `errors=remount-ro` option is a configured ext4 error policy; its presence alone does not show that the policy fired.

## Block-device observations

Command:

```text
lsblk -b -p -o NAME,PATH,PKNAME,TYPE,SIZE,VENDOR,MODEL,SERIAL,TRAN,RO,FSTYPE,LABEL,UUID,MOUNTPOINTS
```

Relevant raw observation, exit `0`:

```text
/dev/sdb    disk 512110190592 SAMSUNG SAMSUNG SSD 830 Series S0Y0NEAC400931 usb 0
/dev/sdb1   part 512108789760                                      0 ext4 blAIne-476GB-SAM 70913570-fb08-4b22-a534-a8e658cb86d4 /run/media/deck/blAIne-476GB-SAM
```

Potentially publication-sensitive serial and USB-path identifiers must be sanitized before public release.

Commands and raw observations:

```text
cat /sys/block/sdb/ro
0

cat /sys/block/sdb/device/vendor
SAMSUNG

cat /sys/block/sdb/device/model
SSD 830 Series

cat /sys/block/sdb/device/state
running

blockdev --getro /dev/sdb
0

blockdev --getro /dev/sdb1
0
```

All six host-visible commands exited `0`. Restricted-namespace attempts to open `/dev/sdb` and `/dev/sdb1` failed because those device nodes were not exposed there; this was a namespace limitation, not a device failure.

`udisksctl info -b /dev/sdb` and `udisksctl info -b /dev/sdb1` both exited `0` in the host namespace and reported `ReadOnly: false`. The partition was identified as ext4, label `blAIne-476GB-SAM`, UUID `70913570-fb08-4b22-a534-a8e658cb86d4`, size `512108789760`, and mounted at `/run/media/deck/blAIne-476GB-SAM`.

`udevadm info` exited `0` for the disk and partition. Relevant observations include USB transport, the `uas` driver, Ugreen enclosure identity, Samsung SSD identity, `ID_FS_TYPE=ext4`, and `UDISKS_AUTO=0`. `UDISKS_AUTO=0` does not itself mean read-only.

### Interpretation

The kernel block layer, UDisks, and block-device ioctl all contradict a hardware or transport write-protect state.

## Mount configuration

Command:

```text
rg -n -i 'sdb|70913570|blAIne-476GB-SAM' /etc/fstab
```

Raw observation: no matching entry; exit `1`, which is the normal no-match status for `rg`.

Interpretation: no matching static fstab rule was observed. This does not identify every possible userspace mounting policy.

## Kernel and journal evidence

### Current attachment cycle

Command:

```text
journalctl -k -b --since '2026-07-14 17:24:00' --no-pager
```

Relevant raw observations, exit `0`:

```text
Jul 14 17:25:54 ... Product: Ugreen Storage Device
Jul 14 17:25:54 ... scsi host1: uas
Jul 14 17:25:54 ... [sdb] 1000215216 512-byte logical blocks: (512 GB/477 GiB)
Jul 14 17:25:54 ... [sdb] Write Protect is off
Jul 14 17:25:54 ... sdb: sdb1
Jul 14 17:25:57 ... EXT4-fs (sdb1): mounted filesystem 70913570-fb08-4b22-a534-a8e658cb86d4 r/w with ordered data mode. Quota mode: none.
```

A time-bounded full-system journal query also reported:

```text
Jul 14 17:25:55 ... blAIne-476GB-SAM: recovering journal
Jul 14 17:25:56 ... blAIne-476GB-SAM: clean, 5060/31260672 files, 2272417/125026560 blocks
Jul 14 17:25:57 ... udisksd: Mounted /dev/sdb1 at /run/media/deck/blAIne-476GB-SAM on behalf of uid 1000
```

No later kernel error, ext4 read-only remount, USB reset, or offline event was present for the current attachment in the bounded journal interval through 17:40. The host namespace remained `rw` when checked at 17:33:22.

### Historical current-boot events

Command:

```text
journalctl -k -b --no-pager | rg -n -i 'sdb|sdb1|EXT4-fs|filesystem error|remount-ro|I/O error|Buffer I/O|blk_update_request|write protect|read-only|USB reset|uas|usb-storage|device offline'
```

Raw observations included repeated earlier events across dynamically reused device names:

- `uas_eh_abort_handler` and UAS device-reset activity;
- `device offline error` on read and write operations;
- `Buffer I/O error`;
- `JBD2: I/O error when updating journal superblock`;
- `Synchronize Cache(10) failed`;
- ext4 journal aborts and shutdown requests;
- `hostbyte=DID_NO_CONNECT` in an earlier USB attachment cycle;
- repeated reports that write protection was off.

The same ext4 UUID was logged with a journal I/O failure at 16:26:23 when the device had the dynamic name `/dev/sda1`; it was disconnected, then attached as `/dev/sdb1` and mounted read/write at 17:25:57. These historical events support a real intermittent USB-path or device-availability problem. They do not show that the current host mount was remounted read-only.

### Limitations

- `dmesg` exited `1`: `read kernel buffer failed: Operation not permitted`.
- The current-boot kernel journal was accessible and supplied relevant messages, so inaccessible `dmesg` is not treated as proof of absence.
- `smartctl` was installed, but `smartctl -i -H -A /dev/sdb` exited `2` because device identity access was not permitted through the USB path without additional privilege/device handling. No SMART conclusion is made.
- No physical cable, enclosure, power, or alternate-port test was authorized.
- No filesystem check or write test was run.

## Internal ext4 alternative observation

Commands:

```text
findmnt -T /home/deck -o SOURCE,TARGET,FSTYPE,OPTIONS
df -B1 -T /home/deck
```

Raw observations, exit `0`:

```text
SOURCE                TARGET     FSTYPE OPTIONS
/dev/nvme0n1p8[/deck] /home/deck ext4   rw,nosuid,nodev,relatime

Filesystem       Type     1B-blocks         Used     Available Use%
/dev/nvme0n1p8   ext4 1982522322944 812574490624 1169931055104 41%
```

Interpretation: `/home/deck` remains a documented internal Linux-native fallback candidate. No runtime or staging location was selected or created by this audit.

## Root-cause classification

| Classification | Status | Evidence-based assessment |
|---|---|---|
| 1. Explicitly mounted read-only | Contradicted for the host; supported only inside the restricted inspection namespace | Host `findmnt`, mountinfo, and `/proc/mounts` all show `rw`. The original `ro` observation is reproduced only in the restricted namespace. |
| 2. Filesystem remounted read-only after an ext4 error | Contradicted for the current attachment | Current attachment logged a journal recovery, clean result, and read/write mount. No later current-cycle remount/error was observed, and the host remained `rw`. Historical journal errors are recorded separately. |
| 3. Device or transport reporting read-only/write-protected | Contradicted | sysfs `ro=0`, both `blockdev --getro` results `0`, UDisks `ReadOnly: false`, and kernel `Write Protect is off`. |
| 4. USB enclosure, cable, power, or I/O failure | Supported as a historical/recurrent failure class; exact component unresolved | Current-boot history contains UAS resets, offline errors, DID_NO_CONNECT, I/O errors, and cache-sync failures. No authorized physical isolation test identifies whether the SSD, enclosure, cable, port, hub, or power path is responsible. This history does not explain a host read-only mount because the host mount is `rw`. |
| 5. Insufficient evidence because host logs or privileged state are inaccessible | Unresolved only for exact hardware-failure localization | Host journal and device state were sufficient to resolve the reported read-only condition as namespace-specific. SMART and physical-path evidence were unavailable, so the cause of historical USB failures remains unresolved. |

## Conclusion

The proposed staging drive was not host-mounted read-only during this diagnosis. The apparent read-only state was created by the restricted execution namespace used for the initial inspection. The current host mount is read/write, the block device is not write-protected, and no current-cycle ext4 error remount was observed.

Separately, historical current-boot evidence demonstrates intermittent USB/UAS I/O and disconnect failures affecting this storage path. That reliability issue warrants a later, separately authorized investigation before the drive is trusted with unique data. It does not authorize repair, remounting, or scaffold creation in this step.

The safest next authorized action is a new, narrowly scoped scaffold-creation run with host-write access limited to `/run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/`, preceded by host-namespace `rw` and nonexistence checks. The project remains `S0 — Proposed`.

