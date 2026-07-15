# RUN-0007 — SteamOS rootless Docker prerequisite audit execution

## Record control

- Identifier: `RUN-0007`
- Execution date: 2026-07-14
- Audit start: 2026-07-14T18:47:39-07:00
- Evidence collection through: 2026-07-14T18:56:34-07:00
- Working directory: `/home/deck`
- Authoritative root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Lifecycle before and after: `S0 — Proposed`
- Authorization: read-only host prerequisite audit plus creation of exactly `AUD-0003`, `RUN-0007`, and `VAL-0007`

## Authorization boundary

The operation was limited to read-only host/staging inspection, immediate non-persistent `unshare` tests, and three Markdown records in the authoritative governance root. It did not authorize package or software changes, service transitions, SteamOS read-only changes, subordinate-ID edits, sysctl writes, systemd-unit edits, lingering changes, firewall changes, Docker context/storage changes, downloads, source acquisition, mounts, storage changes, or staging writes.

## Pre-write gates

| Gate | Command/evidence | Result |
|---|---|---|
| Root exists and structure matches | `realpath`, `find`, `ls`, `stat`, expected-directory loop | PASS |
| Host mount authoritative and writable | host-view `findmnt -T ROOT`; `test -w ROOT` | PASS: `/dev/sda1`, exFAT, `rw`, shared; test writable yes |
| Not staging root | `realpath` and device/inode comparison | PASS: `/dev/sda1` versus `/dev/sdb1`; distinct paths/inodes |
| Not internal root | source comparison with `findmnt -T /` | PASS: `/dev/sda1` versus `/dev/nvme0n1p4` |
| Target names unused | `test -e` for all three targets at initial and final gates | PASS: all absent |
| Lifecycle | `GOVERNANCE.md`, `README.md`, requirements inspection | PASS: S0 — Proposed |
| Git state | `git -C ROOT rev-parse --is-inside-work-tree` | not a Git repository |
| Existing checksum boundary | sorted `sha256sum` pipeline over 63 files | `af95c46abf47da5082affbfa64bf4129cffa19f72fc9de0a38b4e029695147e9` |
| Staging checksum boundary | same method over 147 files | `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f` |
| New storage errors | bounded `journalctl -k -b --since 18:47:39 | rg ...` | PASS: 0 through the pre-write gate and after AUD-0003 creation |

The ordinary restricted view showed both external mounts `ro,private,slave`; the authorized host view showed both `rw,shared`. No mount operation was run.

## Commands executed and outcomes

The following is the command-level execution inventory. Loops expanded only the named paths, binaries, or units. Read-only commands that returned expected no-match or unavailable statuses are preserved.

### Gate, format, and integrity commands

```text
pwd
rg --files -g 'AGENTS.md' /home/deck /run/media/deck/blAIneXPLAT/azerothcore
findmnt -T AUTH_ROOT -o TARGET,SOURCE,FSTYPE,OPTIONS,PROPAGATION
findmnt -T STAGING_ROOT -o TARGET,SOURCE,FSTYPE,OPTIONS,PROPAGATION
ls -ld AUTH_ROOT STAGING_ROOT
stat -c ... AUTH_ROOT STAGING_ROOT
test -e TARGET_RECORD
find AUTH_ROOT -maxdepth 2 ...
sed -n ... GOVERNANCE.md README.md prior AUD/RUN/VAL records
rg -n -i 'checksum|sha256|lifecycle|S0|validate|validation' AUTH_ROOT
git -C AUTH_ROOT rev-parse --is-inside-work-tree
find AUTH_ROOT -type f -print0 | sort -z | xargs -0 sha256sum | sha256sum
find AUTH_ROOT -type f -printf ... | sort | sha256sum
find STAGING_ROOT -type f -print0 | sort -z | xargs -0 sha256sum | sha256sum
find STAGING_ROOT -type f -printf ... | sort | sha256sum
```

All meaningful gates exited `0`. `rg --files` found no applicable `AGENTS.md` in the authoritative tree. Git detection reported `GIT_REPOSITORY=NO`. No checksum manifest was written. The first three-file patch was rejected before application because one Markdown line lacked the patch prefix; exit `1`. An immediate check proved all three names absent, file count still 63, and no content change. Smaller atomic patches were then used.

### Host and SteamOS commands

```text
date --iso-8601=seconds
timedatectl
hostname
hostnamectl
whoami
id
uname -a
uname -m
cat /etc/os-release
printenv PATH HOME XDG_RUNTIME_DIR SHELL XDG_SESSION_TYPE DESKTOP_SESSION XDG_CURRENT_DESKTOP WAYLAND_DISPLAY DISPLAY DBUS_SESSION_BUS_ADDRESS
getent passwd $(id -u) | cut -d: -f7
loginctl show-session ...
systemd-detect-virt
readlink /proc/self/ns/mnt
readlink /proc/1/ns/mnt
findmnt -T PATH -o TARGET,SOURCE,FSTYPE,OPTIONS
steamos-readonly status
steamos-devmode status
test -w PATH; stat -c ... PATH
rg -n '^[[:space:]]*[^#[:space:]]' /etc/pacman.conf
pacman-conf DBPath
pacman -Q pacman
pacman -Q | wc -l
find /usr/lib/holo/pacmandb/local ... | sort | sha256sum
lsattr -d / /usr /etc
```

Relevant statuses: all primary identity/OS queries exited `0`; `hostname` was not installed (127), so hostnamectl supplied `steamdeck`; `systemd-detect-virt` returned no virtualization (1); `readlink /proc/1/ns/mnt` was denied/unavailable (1); a single multi-target `findmnt` invocation was invalid (1) and was corrected with one target per command; `steamos-devmode status` required root (1). `steamos-readonly status` returned `enabled` repeatedly with exit `0`.

### Runtime and prerequisite commands

```text
command -v NAME
pacman -Qo EXECUTABLE
EXECUTABLE --version
pgrep -a [-u 1000] 'dockerd|containerd|podman|rootlesskit'
systemctl show UNIT -p LoadState -p ActiveState -p SubState -p UnitFileState
systemctl --user show UNIT -p LoadState -p ActiveState -p SubState -p UnitFileState
find USER_AND_SYSTEM_BIN_DIRS -maxdepth 1 -name 'dockerd-rootless*.sh'
pacman -Qq | rg '^(docker|docker-compose|docker-buildx|containerd|runc|rootlesskit|slirp4netns|podman|distrobox|buildah|nerdctl|fuse-overlayfs|passt)$'
find /var/lib/docker -mindepth 1 -maxdepth 2 -printf ...
du -sx --block-size=1 /var/lib/docker
```

Discovery/version/package queries exited `0` where components existed. Expected absence tests produced no executable. Both `pgrep` commands returned 1 (no process). All systemctl state queries were read-only. The pre-existing empty `/var/lib/docker` mount was only inspected.

### Subordinate-ID and namespace commands

```text
awk -F: -v u=deck '$1==u {...}' /etc/subuid
awk -F: -v u=deck '$1==u {...}' /etc/subgid
awk range/overlap checks over /etc/subuid and /etc/subgid
sha256sum /etc/subuid /etc/subgid
cat /proc/sys/user/max_*_namespaces
cat /proc/sys/kernel/unprivileged_userns_clone
zgrep -E '^(CONFIG_USER_NS|CONFIG_NAMESPACES|CONFIG_CGROUPS|CONFIG_CGROUP_BPF|CONFIG_OVERLAY_FS|CONFIG_FUSE_FS|CONFIG_SECCOMP)=' /proc/config.gz
unshare --user --map-root-user true
unshare --user --map-auto true
unshare --user --map-root-user sh -c 'id; read uid_map; read gid_map'
```

All three `unshare` commands exited `0` and immediately terminated. The optional `kernel.unprivileged_userns_clone` file was absent/unreadable; the kernel config and real tests supplied positive evidence. No file or daemon was created.

### User-manager and cgroup commands

```text
stat /run/user/1000 /run/user/1000/bus
systemctl --user is-system-running
systemctl --user show -p Version -p State -p Architecture -p ControlGroup
systemctl --user list-units --type=service --state=running --no-legend --no-pager
loginctl show-user deck -p UID -p GID -p Name -p State -p Sessions -p RuntimePath -p Linger
loginctl list-sessions --no-legend
systemctl show user@1000.service -p ...Delegate...
systemctl show session-3.scope -p ...Delegate...
findmnt -T /sys/fs/cgroup -o TARGET,SOURCE,FSTYPE,OPTIONS
stat -fc %T /sys/fs/cgroup
cat cgroup.controllers cgroup.subtree_control /proc/self/cgroup /proc/1/cgroup
test -w CGROUP_FILE; cat cpu.max memory.max pids.max
```

All primary manager/session/cgroup queries exited `0`. One optional user-slice property command placed `--` and `-p` incorrectly (first attempt exit 1; second parsed `-p` as unit names despite exit 0); it is excluded from findings. Valid `user@1000.service` and cgroup-filesystem queries establish delegation.

### Storage, network, resources, and journal commands

```text
findmnt -T /home/deck ...; stat -f /home/deck
test -e /home/deck/.local/share/docker
modinfo overlay
df -B1 -T PATH; df -i -T PATH
lscpu; lscpu -p=CPU,CORE,SOCKET,NODE,ONLINE
free -b
swapon --show --bytes
zramctl --bytes
uptime
cat /sys/class/thermal/thermal_zone*/{type,temp}; sensors
command -v powerprofilesctl tuned-adm ryzenadj
ip -brief link; ip -brief address; ip -4 route; ip -6 route; ip route get 1.1.1.1
ss -H -lntp; ss -H -lnup
systemctl show nftables.service firewalld.service ufw.service ...
nft list ruleset; iptables -S; ip6tables -S
firewall-cmd --state; firewall-cmd --get-active-zones; firewall-cmd --get-default-zone; firewall-cmd --list-all
sysctl net.ipv4.ip_unprivileged_port_start
lsblk -b -p -o NAME,PATH,PKNAME,TYPE,SIZE,VENDOR,MODEL,SERIAL,TRAN,RO,FSTYPE,LABEL,UUID,MOUNTPOINTS /dev/sda /dev/sdb
cat /sys/block/{sda,sdb}/{ro,removable,device/vendor,device/model,device/serial,device/state}
journalctl -k -b --no-pager | rg -i STORAGE_PATTERN
journalctl -k -b --since '2026-07-14 18:47:39' --no-pager | rg -i STORAGE_PATTERN
```

Primary read-only commands exited `0`. The first `swapon --show --bytes --output=...` form was incompatible (1); corrected `swapon --show --bytes` exited `0`. Raw nft query failed unprivileged (1), and iptables/ip6tables queries failed on lock permissions (4); read-only firewalld queries all exited `0`. Power-profile interfaces were unavailable. No network packet was sent by `ip route get`; it was a kernel route lookup. No mount, storage, socket, firewall, or service operation was performed.

## Files created

Exactly these authorized records were created:

1. `records/audits/AUD-0003-steamos-rootless-docker-prerequisite-audit.md`
2. `records/executions/RUN-0007-steamos-rootless-docker-prerequisite-audit.md`
3. `records/validations/VAL-0007-steamos-rootless-docker-prerequisite-audit-validation.md`

No pre-existing project file was amended. The validation record is finalized only with post-write read-only evidence.

## Files and state not changed

- No file outside the authoritative governance root was created or intentionally modified.
- The staging root was read only.
- SteamOS read-only state, developer mode, pacman configuration/database, `/etc/subuid`, `/etc/subgid`, sysctls, systemd units, user lingering, shell profiles, firewall rules, Docker contexts, storage, mounts, and project configuration were not changed.
- No service was enabled, disabled, started, stopped, or restarted.
- `/home/deck/.local/share/docker` remained absent.
- Pre-existing `/var/lib/docker` remained an empty OS-managed mount and was not used.
- No Docker/AzerothCore/module/addon/client/database/container content was acquired.

## Stop conditions

All pre-write stop gates passed. No new relevant kernel storage errors appeared before writing or after AUD-0003 creation. No check requested installation, download, disabling read-only mode, or credential output. Selected environment variables were queried rather than an unrestricted environment dump to avoid exposing secrets.

If any post-write gate detects a new storage error, read-only authoritative mount, collision/replacement, or change outside the three records, validation must fail and work must stop.

## Evidence that no download or installation occurred

The command inventory contains no package transaction, installer, rootless setup script, clone, pull, fetch, download, or container/image command. `curl`, `wget`, and `git` were invoked only with local version flags. `pacman` was used only for `-Q`, `-Qo`, and configuration/local-database reads. Docker and its rootless scripts were absent and therefore could not run. Post-write validation compares the pacman local-database digest/count and verifies that no acquired project/staging payload appeared.

## Post-write validation execution

At 2026-07-14T19:07:34-07:00, the first comprehensive comparison passed every project, staging, package, subordinate-ID, SteamOS, service, firewall, Docker-path, mount, lifecycle, and storage-error check except a manually encoded expected epoch for the already existing `/var/lib/docker` directory. The check expected `1783908210`, which `date -d` showed was one hour earlier than the recorded wall-clock time. This validation invocation exited `1`; it did not modify anything.

Read-only `date -d`, `stat`, `find`, `du`, and `findmnt` checks established that the recorded `2026-07-12 20:03:30.268137770 -0700` mtime/ctime corresponds to epoch `1783911810`; the directory remained empty, 4,096 bytes, root-owned mode 0755, on the same OS-managed mount. The corrected full comparison ran at 2026-07-14T19:08:41-07:00 and exited `0` with every check passing. VAL-0007 preserves the correction and exact comparison results.

Finalization used only authorized patch updates to RUN-0007 and VAL-0007, followed by a read-only closing validation. No pre-existing record or other project file was amended.

## Execution conclusion

The evidence audit completed through record authoring without an installation or acquisition. Its finding is **partially satisfied**, not architecture approval. Lifecycle remains `S0 — Proposed`.
