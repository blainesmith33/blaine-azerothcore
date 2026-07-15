# AUD-0001: Initial Steam Deck Baseline

Status: **Completed read-only audit; pending human review**  
Lifecycle status: **S0 — Proposed**  
Platform scope: **Steam Deck Linux only**  
Evidence start: **2026-07-14T17:03:57-07:00 (MST, UTC-07:00)**  
Evidence completion: **2026-07-14T17:13:48-07:00**  
Authoritative root: **`/run/media/deck/blAIneXPLAT/azerothcore/`**  
Execution record: **RUN-0003**  
Validation record: **VAL-0003**

## Audit method and evidence boundary

Commands were executed from `/home/deck` through a restricted Codex execution environment. The environment exposes a constrained mount and network namespace: many host mounts appear VFS read-only, system and user D-Bus are blocked, netlink is blocked, and `/proc` networking exposes loopback only. These restrictions are audit facts and limitations; they must not be generalized into claims that the host lacks networking, services, or writable mounts.

Each section distinguishes:

- **Commands:** exact or mechanically repeated read-only commands.
- **Raw observations:** command-produced values, without architectural approval.
- **Interpretation:** evidence-based assessment or explicitly labeled inference.
- **Limitations:** facts the current environment could not establish.

Host name, user name, device serials, network addresses, interface identifiers, and similar machine identifiers are **publication-sensitive and require sanitization** before any public release.

## A. Platform identity

### Commands

```bash
date --iso-8601=seconds
date '+%Z %z'
hostname
hostnamectl
cat /proc/sys/kernel/hostname
cat /etc/os-release
for f in /etc/steamos-release /etc/steamos-version /etc/build-id; do
    if [ -r "$f" ]; then printf '%s\n' "--- $f"; cat "$f"; fi
done
uname -a
uname -m
plasmashell --version
printf 'XDG_CURRENT_DESKTOP=%s\nDESKTOP_SESSION=%s\nKDE_FULL_SESSION=%s\nXDG_SESSION_TYPE=%s\nXDG_SESSION_ID=%s\n' \
  "${XDG_CURRENT_DESKTOP-}" "${DESKTOP_SESSION-}" "${KDE_FULL_SESSION-}" \
  "${XDG_SESSION_TYPE-}" "${XDG_SESSION_ID-}"
loginctl show-session "$XDG_SESSION_ID" -p Name -p Type -p Class -p State -p Remote -p Desktop
id
groups
sudo -n true
findmnt -n -o SOURCE,TARGET,FSTYPE,OPTIONS /
test -w / && echo ROOT_DIRECTORY_WRITABLE_BY_USER=yes || echo ROOT_DIRECTORY_WRITABLE_BY_USER=no
pacman -Q plasma-workspace kwin
```

### Raw observations

```text
Evidence time: 2026-07-14T17:03:57-07:00
Timezone output: MST -0700
Hostname: steamdeck [PUBLICATION-SENSITIVE: SANITIZE]

NAME="SteamOS"
VERSION_CODENAME=holo
VARIANT_ID=steamdeck
VERSION_ID=3.8.14
BUILD_ID=20260703.1
STEAMOS_DEFAULT_UPDATE_BRANCH=stable

Kernel: Linux steamdeck 6.16.12-valve24.4-1-neptune-616-gfe145653a794
Kernel build: #1 SMP PREEMPT_DYNAMIC Fri, 03 Jul 2026 23:50:33 +0000
Architecture: x86_64

plasma-workspace 6.4.3-1.4
kwin 6.4.3-1.12
XDG_CURRENT_DESKTOP=KDE
DESKTOP_SESSION=plasma
KDE_FULL_SESSION=true
XDG_SESSION_TYPE=wayland
XDG_SESSION_ID=3

uid=1000(deck) gid=1000(deck) groups=1000(deck),65534(nobody)
[PUBLICATION-SENSITIVE: SANITIZE USER AND GROUP DETAILS]

Root mount: /dev/nvme0n1p4 / btrfs ro,nosuid,nodev,relatime,...
ROOT_DIRECTORY_WRITABLE_BY_USER=no
steamos-readonly status: enabled
```

### Interpretation

- The KDE/Plasma/Wayland environment variables strongly indicate Desktop Mode during evidence collection.
- SteamOS root is observed as Btrfs and read-only; `steamos-readonly status` independently reported `enabled`.
- KDE runtime package version is 6.4.3. `plasmashell --version` did not provide a runtime version because it aborted.
- User identity is `deck` UID/GID 1000 in this audit view. Public material must sanitize it.

### Limitations and failures

- `hostname` was absent (`127`); `/proc/sys/kernel/hostname` and `uname` provided the host name instead.
- `hostnamectl` and `loginctl` could not connect to the system bus (`1`).
- `plasmashell --version` aborted with exit `134`; a coredump may have been attempted by the operating system. No deliberate file was created by the audit.
- `sudo -n true` exited `1` because the restricted execution environment set `no_new_privs` and mapped `/etc/sudo.conf` ownership unexpectedly. Therefore neither passwordless nor password-based sudo capability is verified. No password was requested or exposed.

## B. CPU, memory, and graphics

### Commands

```bash
lscpu
nproc
free -b
swapon --show --bytes
for f in /sys/class/drm/card*/device/uevent; do [ -r "$f" ] && cat "$f"; done
lspci -nnk -s 04:00.0
lspci -nnk
glxinfo -B
vulkaninfo --summary
vainfo
pacman -Q mesa lib32-mesa vulkan-radeon vulkan-tools
for f in /sys/class/thermal/thermal_zone*/type \
         /sys/class/thermal/thermal_zone*/temp \
         /sys/class/power_supply/*/type \
         /sys/class/power_supply/*/capacity \
         /sys/class/power_supply/*/status; do
    [ -r "$f" ] && printf '%s=' "$f" && cat "$f"
done
sensors
for f in /sys/devices/virtual/dmi/id/product_name \
         /sys/devices/virtual/dmi/id/product_version \
         /sys/devices/virtual/dmi/id/board_name \
         /sys/devices/virtual/dmi/id/board_vendor \
         /sys/devices/virtual/dmi/id/bios_version; do
    [ -r "$f" ] && printf '%s=' "$f" && cat "$f"
done
```

### Raw observations

```text
Hardware product/board: Valve Galileo, version 1
BIOS: F7G0114
CPU: AMD Custom APU 0932
Architecture: x86_64
Sockets: 1
Physical cores: 4
Threads per core: 2
Logical CPUs: 8
Maximum reported CPU frequency: 3501.2500 MHz

Memory total:       15,524,212,736 bytes
Memory available:   11,078,832,128 bytes
Swap total:          7,762,079,744 bytes
Swap used:           1,302,970,368 bytes (free output)
Swap device: /dev/zram0, priority 100

GPU: AMD/ATI Sephiroth [AMD Custom GPU 0405], PCI 1002:1435
Kernel driver: amdgpu
mesa 25.3.0.213835.radeonsi_25.3.0-4.1
lib32-mesa 25.3.0.213835.radeonsi_25.3.0-4.1
vulkan-radeon 26.0.0_devel.214836.steamos_25.11.14-1
vulkan-tools 1.4.321.0-1

Instantaneous sensors:
- ACPI thermal zone: 87.0 C
- AMD GPU edge: 70.0 C
- NVMe composite: 61.9 C
- Wi-Fi sensor: 64.0 C
- System fan: 6465 RPM
- GPU/APU power reading: approximately 15 W
- Battery: 99%, not charging; battery sensor 34.0 C
```

### Interpretation

- Four physical cores/eight threads and approximately 15.5 GB RAM are sufficient to define and investigate multiple realms, but not evidence that multiple worldservers can run concurrently at acceptable performance.
- ZRAM swap was already in active use at the observation time, so memory headroom must be measured under later workloads.
- Sensor values are instantaneous and were collected without load testing; they are not thermal limits or benchmark results.

### Limitations

- `glxinfo -B` could not open display `:0` (`127` wrapper result).
- `vulkaninfo --summary` failed to enumerate a GPU (`127` wrapper result).
- `vainfo` was unavailable (`127`). Installed Mesa/Vulkan packages confirm software presence, not working Vulkan capability in this environment.

## C. Storage inventory

### Commands

```bash
lsblk -b -p -o NAME,PATH,PKNAME,TYPE,SIZE,VENDOR,MODEL,SERIAL,TRAN,RM,HOTPLUG,PTTYPE,PARTTYPE,PARTLABEL,FSTYPE,LABEL,UUID,MOUNTPOINTS
findmnt -o SOURCE,TARGET,FSTYPE,OPTIONS
df -B1 -T
findmnt -T /run/media/deck/blAIneXPLAT -o SOURCE,TARGET,FSTYPE,OPTIONS
findmnt -T /run/media/deck/blAIneXPLAT -n -o FSROOT,ID,PROPAGATION,VFS-OPTIONS,FS-OPTIONS
df -B1 -T /run/media/deck/blAIneXPLAT
stat -c 'path=%n mode=%A octal=%a owner=%U uid=%u group=%G gid=%g size=%s type=%F' \
  /run/media/deck/blAIneXPLAT /run/media/deck/blAIneXPLAT/azerothcore
stat -f -c 'path=%n fs_type=%T block_size=%S blocks=%b blocks_free=%f blocks_available=%a files=%c files_free=%d max_name_length=%l' \
  /run/media/deck/blAIneXPLAT
namei -l /run/media/deck/blAIneXPLAT/azerothcore
find /run/media/deck/blAIneXPLAT/azerothcore -maxdepth 2 -printf '%y %M %u %g %s %p\n' | sort
modinfo exfat
fsck.exfat -V
```

### Raw block-device observations

```text
/dev/sda
  Type: disk; GPT; USB; hotplug
  Capacity: 1,024,209,543,168 bytes
  Vendor/model: Sabrent / Micron_2400_MTFDKBK1T0QFM
  Serial: 234544AAB642 [PUBLICATION-SENSITIVE: SANITIZE]
  /dev/sda1
    Capacity: 1,024,208,477,696 bytes
    GPT type: Microsoft Basic Data
    PARTLABEL: blAIneXPLAT
    Filesystem: exfat
    LABEL: blAIneXPLAT
    UUID: 6E5E-D1B2 [PUBLICATION-SENSITIVE: REVIEW/SANITIZE]
    Mount: /run/media/deck/blAIneXPLAT

/dev/nvme0n1
  Type: disk; GPT; NVMe
  Capacity: 2,000,398,934,016 bytes
  Model: Corsair MP600 CORE MINI
  Serial: A7SIB50600BRSA [PUBLICATION-SENSITIVE: SANITIZE]
  p1 268,435,456 vfat esp mounted /esp
  p2 67,108,864 vfat efi-A mounted /efi
  p3 67,108,864 vfat efi-B not mounted
  p4 5,368,709,120 btrfs rootfs-A mounted /
  p5 5,368,709,120 btrfs rootfs-B not mounted
  p6 268,435,456 ext4 var-A mounted /var
  p7 268,435,456 ext4 var-B not mounted
  p8 1,988,720,922,624 ext4 home mounted /home and offload paths

/dev/zram0
  Capacity: 7,762,083,840 bytes
  Filesystem: swap; mounted [SWAP]
```

### Raw filesystem-capacity observations

```text
Filesystem/mount                         Type     Capacity (bytes)     Used             Available
/dev/nvme0n1p4 /                         btrfs      5,368,709,120     3,718,778,880     1,406,775,296
/dev/nvme0n1p6 /var                      ext4         241,081,344        38,065,152       185,400,320
/dev/nvme0n1p8 /home                     ext4   1,982,522,322,944   811,031,076,864 1,171,474,468,864
/dev/sda1 /run/media/deck/blAIneXPLAT    exfat  1,024,175,898,624        11,403,264 1,024,164,495,360

Project mount (combined OPTIONS view):
ro,nosuid,nodev,relatime,uid=1000,gid=1000,fmask=0022,dmask=0022,iocharset=utf8,errors=remount-ro

Detailed split:
VFS-OPTIONS=ro,nosuid,nodev,relatime
FS-OPTIONS=rw,uid=1000,gid=1000,fmask=0022,dmask=0022,iocharset=utf8,errors=remount-ro

Visible ownership/mode:
/run/media/deck/blAIneXPLAT              drwxr-xr-x 0755 deck:deck
/run/media/deck/blAIneXPLAT/azerothcore drwxr-xr-x 0755 deck:deck
Observed exFAT allocation block size: 131072 bytes
Kernel exFAT module: in-tree GPL module for kernel 6.16.12-valve24.4-1-neptune...
exfatprogs version observed: 1.2.9
```

### Governed-project filesystem-operation evidence

- Normal directory and file **creation** is demonstrated by RUN-0001 and RUN-0002 and their validated inventories.
- Existing Markdown **content replacement/modification** is demonstrated by RUN-0002 narrow amendments.
- Normal **deletion**, standalone **rename**, and explicitly measured **atomic replacement/durability** have not been separately demonstrated by a governed test.
- No destructive or synthetic filesystem test was performed in AUD-0001.

### exFAT source-backed constraints and risks

Primary source consulted on 2026-07-14: [Microsoft exFAT File System Specification](https://learn.microsoft.com/en-us/windows/win32/fileio/exfat-specification).

- The specification defines exFAT as case-insensitive and case-preserving through its up-case table. Case-colliding source trees therefore require explicit validation.
- Standard file attributes are DOS-style ReadOnly, Hidden, System, Directory, and Archive flags; they do not define native POSIX UID/GID, mode bits, executable bits, symbolic links, Unix sockets, or other Unix inode types.
- The observed Linux mount supplies one synthetic `uid`, `gid`, `fmask`, and `dmask` for the volume. The visible `0755` modes—including executable bits on Markdown files—are therefore not evidence of per-file native Unix permission semantics.
- Source builds that rely on symlinks, per-file executable state, case-distinct names, or Unix-special files are at risk and require controlled validation.
- Docker/Podman storage drivers and MySQL/MariaDB data directories depend on filesystem semantics beyond simple file creation. exFAT placement is **not approved** without vendor/runtime support review and direct validation of locking, rename/replace, fsync/durability, permissions, sockets, case behavior, migration, backup, and rollback.
- Advisory file locking behavior, atomic rename/replacement guarantees in the intended cross-platform workflow, and crash consistency were not validated here.

### Internal Linux-native candidates—not selected

- `/home/deck` on writable ext4: approximately 1.171 TB available; strongest observed candidate for later runtime/build evaluation.
- `/var/lib/docker` maps to an ext4 offload path on the large home partition, but Docker is absent and this audit namespace exposes it read-only. It is only a candidate subject for later host-side verification.
- `/var` is ext4 but only approximately 185 MB available and read-only in this view; not a practical observed capacity for the planned workloads.
- `/` is Btrfs, read-only, and has approximately 1.4 GB available; it is not selected.

No final runtime location was selected or created.

## D. Existing software and tools

### Command method

```bash
command -v <tool>
<tool> <read-only version option>
pacman -Q <selected-package>
```

### Raw inventory

| Tool | Observed result |
|---|---|
| git | 2.50.1 |
| curl | 8.15.0 |
| wget | 1.25.0 |
| rsync | 3.4.1 |
| tar | GNU tar 1.35 |
| unzip | Info-ZIP 6.00 |
| zip | Info-ZIP 3.0 |
| jq | 1.8.1 |
| yq | Absent |
| cmake | Absent |
| make | Absent |
| ninja | Absent |
| gcc | Absent |
| g++ | Absent |
| clang | Absent |
| docker | Absent; package absent |
| docker compose | Unavailable because Docker is absent |
| docker-compose | Absent; package absent |
| podman | Package 5.5.2-1.1 present; even `--version` failed in the restricted namespace |
| podman-compose | Absent |
| distrobox | 1.8.1.2 |
| systemd-nspawn | systemd 257 (257.7-2.5-arch) |
| flatpak | 1.16.6 |
| python | 3.13.5 |
| python3 | 3.13.5 |
| pip | Absent |
| openssl | 3.5.7 |
| mysql client | Absent |
| mariadb client | Absent |
| sqlite3 | 3.50.3 |
| screen | Absent |
| tmux | 3.5a |
| lsof | 4.99.5 |
| ss | iproute2 6.16.0 |
| netstat | Absent |
| nc | Absent |
| socat | 1.8.0.3 |

Absence is an audit result only; no installation is authorized by this record.

## E. Container-runtime state

### Commands

```bash
command -v docker
pacman -Q docker docker-compose
pgrep -a -f '(^|/)(dockerd|containerd|docker-proxy)( |$)'
stat /run/docker.sock /var/run/docker.sock /run/containerd/containerd.sock /var/lib/docker /etc/docker
find /var/lib/docker -maxdepth 2 -printf '%y %M %u %g %p\n'
systemctl is-active docker.service
command -v podman
pacman -Q podman podman-compose
pgrep -a -f '(^|/)(podman|conmon|crun)( |$)'
stat /run/user/1000/podman/podman.sock /run/podman/podman.sock \
  /home/deck/.local/share/containers /home/deck/.config/containers /etc/containers
find /etc/containers -maxdepth 3 -printf '%y %M %u %g %s %p\n'
systemctl --user is-active podman.socket
find /run/media/deck/blAIneXPLAT/azerothcore -maxdepth 5 -type f \
  \( -iname 'compose.yml' -o -iname 'compose.yaml' \
     -o -iname 'docker-compose.yml' -o -iname 'docker-compose.yaml' \
     -o -iname 'Containerfile' -o -iname 'Dockerfile' \) -print
```

### Raw observations

- Docker command and Arch package: absent.
- Docker daemon/containerd/docker-proxy processes: none visible.
- Docker and containerd sockets: absent.
- `/var/lib/docker`: exists as an empty visible directory mapped to internal ext4 offload storage.
- `/etc/docker`: absent.
- Docker service state: inaccessible because system D-Bus is blocked.
- Docker client/server versions, root directory, storage driver, backing filesystem, containers, images, networks, and volumes cannot be queried because Docker is absent.
- Podman package 5.5.2-1.1 is present; podman-compose is absent.
- `podman --version` attempted `mkdir /run/user/1000/libpod` and failed with read-only-filesystem error. Further Podman commands were skipped to avoid initialization writes.
- No Podman/conmon/crun process or Podman socket was visible.
- User Podman storage/config directories were absent. System `/etc/containers` defaults exist.
- No container definition file exists in the governed project.
- No AzerothCore container asset was found.

### Interpretation and limitation

Docker is not installed or operational based on command/package/process/socket evidence. Podman software is present but its runtime/storage state is not initialized or queryable in this audit environment. The audit does not prove host-side service absence where D-Bus access was blocked.

## F. Existing AzerothCore and WoW assets

### Search scope and commands

Search roots were limited to existing user-accessible locations:

```text
/home/deck/Desktop
/home/deck/Documents
/home/deck/Downloads
/home/deck/.local/share/Steam/steamapps/common
/run/media/deck/blAIneXPLAT
```

`/home/deck/Projects` and `/home/deck/Games` were absent. Commands used bounded `find -xdev -maxdepth 8` name searches, bounded `.git/config` searches, and `rg --max-depth 8 --max-filesize 2M` over YAML, configuration, WTF, SQL, and Markdown files for AzerothCore/authserver/worldserver/acore/ChromieCraft/realmlist terms.

### Raw observations and classification

- **Confirmed AzerothCore assets:** none.
- **Governed project references:** `/run/media/deck/blAIneXPLAT/azerothcore/` and its governance Markdown matched terminology searches; these are documentation, not server assets.
- **Compose files:** none found in the bounded roots.
- **AzerothCore Git repositories:** none found by targeted `.git/config` search.
- **authserver/worldserver configuration:** none found.
- **AzerothCore SQL/database exports:** none found.
- **module source directories:** none found outside the empty governed module structure.
- **WoW 3.3.5a/ChromieCraft clients or executables:** none found.
- **realmlist configuration:** none found.
- **targeted prior backups:** none found.
- **Inaccessible search locations:** no errors were emitted in the chosen user-accessible roots. Root-owned and unrestricted full-disk locations were intentionally not searched.

No discovered asset was modified, adopted, or downloaded.

## G. Network baseline

### Commands

```bash
cat /proc/sys/kernel/hostname
ip -brief link
ip -brief address
ip route show
ip -6 route show
cat /etc/resolv.conf
resolvectl status
ss -lntup
ss -H -lntup | rg ':(3724|8085|8086|8087)(\s|$)'
nft --version
iptables --version
ip6tables --version
firewall-cmd --version
nft list ruleset
iptables -S
ufw status
firewall-cmd --state
nmcli -t -f DEVICE,TYPE,STATE,CONNECTION device status
for n in /sys/class/net/*; do
    cat "$n/type" "$n/operstate"
    test -d "$n/wireless"
    readlink -f "$n/device"
done
cat /proc/net/route
cat /proc/net/if_inet6
sed -n '1,260p' /proc/net/fib_trie
getent ahosts steamdeck
cat /proc/net/tcp /proc/net/tcp6 /proc/net/udp /proc/net/udp6
```

### Raw observations

```text
/etc/resolv.conf:
  Generated by NetworkManager
  nameserver 127.0.0.53
  options edns0 trust-ad

Sysfs interfaces:
  wlan0: wireless=yes, operstate=up, Qualcomm QCNFA765 / ath11k_pci
  enp4s0f3u1u4: wireless=no, operstate=down, USB device path
  lo: loopback

Tools:
  nft present, but netlink initialization denied
  iptables/ip6tables 1.8.11 legacy present
  firewall-cmd present, but D-Bus denied
  ufw absent

Restricted namespace /proc view:
  no default route rows
  only IPv6 loopback
  IPv4 fib contains loopback only
  TCP/UDP tables contain headers only
  no rows for hex ports corresponding to 3724, 8085, 8086, or 8087
```

### Interpretation

- Sysfs indicates that Wi-Fi is the active physical transport and the visible USB Ethernet interface is down.
- The command process is in a restricted network namespace. Its empty socket/route tables **do not establish host port availability or firewall state**.
- Port 3724 and likely worldserver ports 8085–8087 remain unverified on the host.

### Limitations and sanitization

- Netlink denied `ip`, `ss`, and `nft` host state.
- D-Bus denied `resolvectl`, NetworkManager, and firewalld state.
- iptables rules were inaccessible due lock/permission denial.
- No authoritative host IPv4/IPv6 addresses, default routes, upstream DNS servers, listeners, or firewall policy were obtained.
- Hostnames, interface names, MAC addresses, IP addresses, DNS servers, routes, and network identifiers require publication review/sanitization. MAC addresses were not collected.

## H. Multi-realm resource implications

The following are **unverified architectural inferences**, not approvals:

- Multiple realms can be defined as manifests/configuration while stopped. Runtime feasibility is not established.
- Keeping all realms stopped by default is a prudent candidate policy until measured startup, idle, and loaded resource profiles exist.
- Four cores/eight threads, 15.5 GB RAM, active ZRAM use, and observed thermal state suggest CPU, memory, thermal, and database contention will be material when several worldservers run simultaneously.
- The internal ext4 home partition has substantial free capacity, but build size, database size, I/O latency, backup time, and write amplification remain unmeasured.
- One shared authentication service appears logically consistent with ADR-0002, but it requires exact-version database compatibility, failure-domain, migration, backup, security, and realm-list validation.
- Separate worldserver containers or Compose projects appear reasonable isolation candidates, but Docker is absent and Podman is not validated. This is not a runtime selection.
- Different module/core baselines may benefit from separately pinned build images, but image storage, build time, cache size, and cross-platform behavior need measurement.
- Running several worldservers simultaneously requires later measured validation under realistic database and gameplay load; no count is approved.

## I. SteamOS persistence considerations

### Commands

```bash
steamos-readonly status
pacman -Q steamos-atomupd-client steamdeck-kde-presets plasma-workspace flatpak distrobox podman
stat /home/deck /home/deck/.config /home/deck/.local /home/deck/.local/share \
  /home/deck/.config/systemd/user /home/deck/.local/share/flatpak \
  /var/lib/flatpak /var/lib/docker /run/user/1000
findmnt -T <each-path> -n -o SOURCE,TARGET,FSTYPE,OPTIONS
find /home/deck/.config/systemd/user -maxdepth 3 -printf '%y %M %u %g %s %p\n'
systemctl --user list-unit-files --no-pager
flatpak list --columns=application,ref,version,branch,installation
flatpak remotes --show-details
```

### Raw observations

- `steamos-readonly status`: `enabled`.
- `steamos-atomupd-client 0.20260331.1-1` is installed.
- `/home/deck` is writable ext4 on `/dev/nvme0n1p8`.
- `/home/deck/.config/systemd/user` exists; one `gamemoded.service` enablement symlink was visible.
- User-systemd bus query was denied, so unit runtime capability/state is unverified.
- Flatpak 1.16.6 is installed. System applications include Google Chrome and Visual Studio Code plus platform/SDK runtimes.
- A user Flatpak repository/config structure exists under writable `/home/deck/.local/share/flatpak`.
- Distrobox 1.8.1.2 and Podman package 5.5.2 are installed; user container storage was absent.
- `/var/lib/flatpak` and `/var/lib/docker` are offloaded to internal ext4 paths but appear read-only in the audit namespace.
- `/run/user/1000` appears read-only in the audit namespace, blocking Podman initialization.

### Candidate approaches requiring later ADR evidence—not selected

1. Rootless Podman/Distrobox with runtime state on internal ext4 home storage.
2. Native build/runtime under a governed path on internal ext4 with systemd user services.
3. Docker with its already defined internal ext4 offload path, only if a later approved installation and host-side persistence validation support it.
4. Split placement: authoritative cross-platform documents/manifests/backups on exFAT and Linux-native runtime/build/database state on internal ext4.
5. Other SteamOS-compatible approaches identified by later research.

Required evidence includes official SteamOS update/persistence behavior, writable-path guarantees, user-service behavior across reboot/update, container runtime initialization, storage-driver support, database durability, permissions, backup/restore, resource use, and rollback. AUD-0001 selects none of these.

## J. Audit conclusions

### Verified platform facts

- SteamOS 3.8.14, build 20260703.1, stable branch metadata.
- Kernel 6.16.12 Valve Neptune build on x86_64.
- Valve Galileo hardware, AMD Custom APU with 4 cores/8 threads, AMD custom GPU using amdgpu.
- Approximately 15.5 GB RAM plus 7.76 GB ZRAM swap.
- KDE Plasma package 6.4.3 and a KDE Plasma Wayland Desktop Mode session environment.
- SteamOS read-only mode enabled; root Btrfs mounted read-only.
- Writable internal ext4 home path exists with approximately 1.171 TB available.
- Cross-platform project drive is `/dev/sda1`, exFAT, with approximately 1.024 TB available.

### Detected software

Git, curl, wget, rsync, tar, unzip, zip, jq, Flatpak, Python 3, OpenSSL, SQLite, tmux, lsof, ss, socat, Podman package, Distrobox, and systemd-nspawn are present. Exact observed versions appear in section D.

### Missing or unusable software

Docker, Docker Compose, docker-compose, podman-compose, yq, CMake, make, Ninja, GCC/G++, Clang, pip, MySQL/MariaDB clients, screen, netstat, and nc were absent. Podman runtime operation was not usable in this audit namespace.

### Storage constraints and exFAT risks

- The project drive provides cross-platform capacity but lacks native POSIX metadata semantics and is case-insensitive. Symlinks, Unix sockets, executable bits, per-file permissions, locking, database durability, container storage, and atomic replacement behavior require validation.
- Internal ext4 home storage is the primary observed Linux-native candidate; no final runtime location is selected.
- Root and `/var` are read-only in this view; `/var` is also very small.

### Existing conflicting or reusable assets

- No AzerothCore, WoW/ChromieCraft, Compose, realmlist, module-source, SQL, or backup asset was found in the bounded search.
- Podman/Distrobox/Flatpak and the internal ext4 offload layout may be reusable platform capabilities, subject to later validation.

### Network constraints

The audit environment could not see host addresses, routes, sockets, firewall rules, or port use. Active Wi-Fi is visible through sysfs, but ports 3724 and 8085–8087 remain unverified. Host-side network evidence is required before S1 approval.

### Likely Steam Deck resource constraints

CPU, memory, active swap, thermal headroom, internal storage I/O, database load, and simultaneous-worldserver count require later measurements. All realms should remain stopped by default as a candidate policy until those measurements exist; this is not an operational approval.

### Unresolved questions and blockers before S1 approval

1. Host-side sudo capability and intended privilege model.
2. Authoritative host network interfaces, addresses, routes, DNS, listeners, firewall status, and port conflicts.
3. Functional Vulkan/graphics capability if any build/client workflow depends on it.
4. Host-side Podman/Distrobox behavior and whether Docker is desired or supportable.
5. SteamOS update persistence guarantees for proposed paths and user services.
6. exFAT behavior for rename/replace, locking, case collisions, permissions, symlinks, sockets, executable state, durability, and backup/restore.
7. Internal ext4 path, quota/capacity policy, permissions, backup, and rollback design.
8. Measured build, database, authserver, and worldserver resource profiles.
9. Database and client requirements for the eventual pinned AzerothCore/module convergence candidates.

### Recommended later architecture-decision topics

- Persistent Linux-native runtime/build/database placement versus exFAT document/artifact placement.
- Docker versus rootless Podman/Distrobox versus native execution.
- Shared authentication and per-realm database/failure boundaries.
- Instance start/stop defaults and measured concurrency limits.
- Build-image strategy for different exact module baselines.
- Backup/restore, upgrade/rollback, and SteamOS update persistence.
- Host networking, firewall, LAN-only/public exposure policy, and sanitized documentation.

## Final audit statement

AUD-0001 is evidence gathering only. It does **not** establish that Docker, AzerothCore, any MySQL/MariaDB database, a game client, authserver, worldserver, or realm is installed or operational. It does **not** select or approve a final installation architecture, runtime, database, storage location, network exposure, or integration baseline. The project remains **S0 — Proposed** pending human review.
