# RUN-0003: Perform Initial Steam Deck Baseline Audit

Status: **Executed; validated by VAL-0003**  
Lifecycle before and after: **S0 — Proposed**  
Audit start: **2026-07-14T17:03:57-07:00**  
Audit completion: **2026-07-14T17:13:48-07:00**  
Working directory: **`/home/deck`**  
Authoritative project root: **`/run/media/deck/blAIneXPLAT/azerothcore/`**

## Scope

Execute only read-only baseline inspection commands and create AUD-0001, RUN-0003, and VAL-0003. No installation, service change, container operation, source acquisition, database creation, firewall change, mount change, permission change, runtime-directory creation, Git initialization, or later audit was authorized.

## Command-recording convention

Commands were issued through shell batches. A wrapper printed each effective command and its exit status. Compound `for`/`if` pipelines below are recorded as the shell command actually issued; their generated per-tool/package invocations are also enumerated. Raw observations and interpretations are in AUD-0001.

## 1. Non-overwrite gate and governance read

```bash
set -euo pipefail
root=/run/media/deck/blAIneXPLAT/azerothcore
resolved="$(realpath "$root")"
printf 'RESOLVED_ROOT=%s\n' "$resolved"
test "$resolved" = "$root"
aud="$root/records/audits/AUD-0001-initial-steam-deck-baseline.md"
if [ -e "$aud" ]; then
    find "$root/records/audits" -maxdepth 2 \
      -printf '%M %u %g %s %TY-%Tm-%TdT%TH:%TM:%TS%Tz %p\n' | sort
    sed -n '1,400p' "$aud"
    exit 3
fi
for rel in README.md GOVERNANCE.md \
  docs/governance/project-charter.md \
  docs/governance/authority-and-versioning-rules.md \
  docs/governance/change-control.md \
  docs/requirements/acceptance-criteria.md \
  docs/requirements/multi-realm-and-module-integration-requirements.md \
  docs/architecture/multi-realm-architecture.md \
  docs/architecture/module-suite-architecture.md \
  records/decisions/ADR-0001-project-directory-and-platform-separation.md \
  records/decisions/ADR-0002-multi-realm-server-architecture.md \
  records/decisions/ADR-0003-module-version-convergence-strategy.md \
  records/decisions/ADR-0004-modular-blaine-suite-over-monolithic-module.md; do
    sed -n '1,260p' "$root/$rel"
done
```

Result: authoritative root matched, AUD-0001 did not exist, governance was read, exit `0`.

## 2. Platform, CPU, memory, graphics, and thermal command ledger

| Command | Exit |
|---|---:|
| `date --iso-8601=seconds` | 0 |
| `date '+%Z %z'` | 0 |
| `hostname` | 127 |
| `hostnamectl` | 1 |
| `cat /etc/os-release` | 0 |
| read `/etc/steamos-release`, `/etc/steamos-version`, `/etc/build-id` when readable | 0 |
| `uname -a` | 0 |
| `uname -m` | 0 |
| `plasmashell --version` | 134 |
| print `XDG_CURRENT_DESKTOP`, `DESKTOP_SESSION`, `KDE_FULL_SESSION`, `XDG_SESSION_TYPE`, `XDG_SESSION_ID` | 0 |
| `loginctl show-session "$XDG_SESSION_ID" ...` | 1 |
| `id` | 0 |
| `groups` | 0 |
| `sudo -n true` | 1 |
| `findmnt -n -o SOURCE,TARGET,FSTYPE,OPTIONS /` | 0 |
| `test -w / ...` | 0 |
| `lscpu` | 0 |
| `nproc` | 0 |
| `free -b` | 0 |
| `swapon --show --bytes` | 0 |
| read `/sys/class/drm/card*/device/uevent` | 0 |
| `glxinfo -B` guarded by `command -v` | 127 wrapper result |
| `vulkaninfo --summary` guarded by `command -v` | 127 wrapper result |
| `vainfo` guarded by `command -v` | 127 |
| read thermal-zone and power-supply type/temp/capacity/status files | 0 |
| `sensors` guarded by `command -v` | 0 |
| read DMI product/board/vendor/BIOS files | 0 |
| `lspci -nnk -s 04:00.0` | 0 |
| `lspci -nnk` | 0 |
| `pacman -Q plasma-workspace kwin mesa lib32-mesa vulkan-radeon vulkan-tools` | 0 |
| locate `plasmashell` and `kwin_wayland` | 0 |

## 3. Storage command ledger

| Command | Exit |
|---|---:|
| `lsblk -b -p -o NAME,PATH,PKNAME,TYPE,SIZE,VENDOR,MODEL,SERIAL,TRAN,RM,HOTPLUG,PTTYPE,PARTTYPE,PARTLABEL,FSTYPE,LABEL,UUID,MOUNTPOINTS` | 0 |
| `findmnt -o SOURCE,TARGET,FSTYPE,OPTIONS` | 0 |
| `df -B1 -T` | 0 |
| `findmnt -T /run/media/deck/blAIneXPLAT -o SOURCE,TARGET,FSTYPE,OPTIONS` | 0 |
| `df -B1 -T /run/media/deck/blAIneXPLAT` | 0 |
| `stat -c ... /run/media/deck/blAIneXPLAT /run/media/deck/blAIneXPLAT/azerothcore` | 0 |
| `stat -f -c ... /run/media/deck/blAIneXPLAT` | 0 |
| `namei -l /run/media/deck/blAIneXPLAT/azerothcore` | 0 |
| bounded `find` of project to depth 2 with mode/owner/size | 0 |
| `findmnt` and `df` for `/`, `/home`, `/home/deck`, `/var`, `/var/lib`, `/var/lib/docker`, `/var/lib/containers` | 0 |
| `modinfo exfat` | 0 |
| `fsck.exfat -V` guarded by `command -v` | 127 wrapper result; version 1.2.9 observed |
| `findmnt -T /run/media/deck/blAIneXPLAT -n -o FSROOT,ID,PROPAGATION,VFS-OPTIONS,FS-OPTIONS` | 0 |

No file creation, deletion, rename, lock, symlink, socket, mount, fsck repair, benchmark, or destructive filesystem command was run.

## 4. Tool-version inventory ledger

For each tool, the batch ran `command -v <tool>` and, only when present, the listed version command.

| Tool and version command | Result/exit |
|---|---|
| `git --version` | 0 |
| `curl --version` | 0 |
| `wget --version` | 0 |
| `rsync --version` | 0 |
| `tar --version` | 0 |
| `unzip -v` | 0 |
| `zip -v` | 0 |
| `jq --version` | 0 |
| `yq --version` | absent; command-v 1 |
| `cmake --version` | absent; command-v 1 |
| `make --version` | absent; command-v 1 |
| `ninja --version` | absent; command-v 1 |
| `gcc --version` | absent; command-v 1 |
| `g++ --version` | absent; command-v 1 |
| `clang --version` | absent; command-v 1 |
| `docker --version` | absent; command-v 1 |
| `docker compose version` | skipped because Docker absent; 127 recorded |
| `docker-compose --version` | absent; command-v 1 |
| `podman --version` | 1; attempted `/run/user/1000/libpod` creation and failed read-only |
| `podman-compose --version` | absent; command-v 1 |
| `distrobox --version` | 0 |
| `systemd-nspawn --version` | 0 |
| `flatpak --version` | 0 |
| `python --version` | 0 |
| `python3 --version` | 0 |
| `pip --version` | absent; command-v 1 |
| `openssl version -a` | 0 |
| `mysql --version` | absent; command-v 1 |
| `mariadb --version` | absent; command-v 1 |
| `sqlite3 --version` | 0 |
| `screen --version` | absent; command-v 1 |
| `tmux -V` | 0 |
| `lsof -v` | 0 |
| `ss --version` | 0 |
| `netstat --version` | absent; command-v 1 |
| `nc -h` | absent; command-v 1 |
| `socat -V` | 0 |

Selected package queries used `pacman -Q` separately for:

| Package | Exit |
|---|---:|
| plasma-workspace | 0 |
| mesa | 0 |
| vulkan-radeon | 0 |
| vulkan-tools | 0 |
| docker | 1 |
| docker-compose | 1 |
| podman | 0 |
| podman-compose | 1 |
| distrobox | 0 |
| flatpak | 0 |
| git | 0 |
| cmake | 1 |
| gcc | 1 |
| clang | 1 |
| mariadb | 1 |
| mariadb-clients | 1 |
| sqlite | 0 |
| rsync | 0 |
| jq | 0 |
| yq | 1 |

## 5. Container-runtime discovery ledger

| Command | Exit |
|---|---:|
| locate Docker and `pacman -Q docker docker-compose` | compound 0; both packages absent |
| `pgrep` for dockerd/containerd/docker-proxy | 1; none visible |
| `stat` Docker/containerd sockets, `/var/lib/docker`, `/etc/docker` | 0 |
| bounded `find /var/lib/docker -maxdepth 2` | 0 |
| `systemctl is-active docker.service` | 1; system bus denied |
| locate Podman and `pacman -Q podman podman-compose` | compound 0; podman-compose absent |
| `pgrep` for podman/conmon/crun | 1; none visible |
| `stat` Podman sockets and user/system container paths | 0 |
| bounded `find` of existing container config paths | 0 |
| `systemctl --user is-active podman.socket` | 1; user bus denied |
| bounded project `find` for Compose/Containerfile/Dockerfile | 0; no results |

Docker client queries were skipped because Docker was absent. Podman info/list/image/container/network/volume commands were skipped because even `podman --version` attempted initialization and failed.

## 6. Bounded asset-search ledger

The following existing roots were searched: `/home/deck/Desktop`, `/home/deck/Documents`, `/home/deck/Downloads`, `/home/deck/.local/share/Steam/steamapps/common`, and `/run/media/deck/blAIneXPLAT`. `/home/deck/Projects` and `/home/deck/Games` were absent.

| Command | Exit |
|---|---:|
| print present/absent bounded search roots | 0 |
| bounded `find -xdev -maxdepth 8` for AzerothCore/ChromieCraft/WoW/authserver/worldserver/realmlist/executable/SQL names | 0 |
| bounded `find` for Compose filenames | 0 |
| bounded `.git/config` search piped to `rg -i 'azerothcore|chromiecraft'` | 0 |
| bounded `rg --max-depth 8 --max-filesize 2M` over YAML/conf/WTF/SQL/Markdown | 0 |
| bounded targeted backup-name `find` | 0 |

An unrestricted full-disk search was deliberately skipped as excessively broad and outside the reasonable user-accessible scope.

## 7. Network ledger

| Command | Exit |
|---|---:|
| `cat /proc/sys/kernel/hostname` | 0 |
| `ip -brief link` | 1; netlink denied |
| `ip -brief address` | 1; netlink denied |
| `ip route show` | 1; netlink denied |
| `ip -6 route show` | 1; netlink denied |
| `cat /etc/resolv.conf` | 0 |
| `resolvectl status` guarded by command-v | 1; system bus denied |
| `ss -lntup` | 0 with netlink denial and header only |
| `ss ... | rg` for ports 3724/8085/8086/8087 | 1; unavailable/no rows |
| locate/version nft, iptables, ip6tables, ufw, firewall-cmd | compound 0 |
| `nft list ruleset` | 1; netlink denied |
| `iptables -S` | 1; lock permission denied |
| `ufw status` | 1; absent |
| `firewall-cmd --state` | 1; D-Bus denied |
| `nmcli ... device status` | 1; NetworkManager access denied |
| read sysfs interface type/operstate/wireless/device | 0 |
| `cat /proc/net/route` | 0; header only |
| `cat /proc/net/if_inet6` | 0; loopback only |
| `sed -n '1,260p' /proc/net/fib_trie` | 0; loopback only |
| `getent ahosts steamdeck` | 0; loopback mappings |
| read `/proc/net/tcp`, `tcp6`, `udp`, `udp6` | 0; headers only |
| scan proc socket tables for hex ports 0E8C/1F95/1F96/1F97 | 0; no rows in restricted namespace |

No interface, route, address, DNS, firewall, NAT, port-forward, listener, or service was modified.

## 8. Persistence ledger

| Command | Exit |
|---|---:|
| `steamos-readonly status` | 0 |
| `pacman -Q steamos-customizations steamos-atomupd-client steamdeck-kde-presets plasma-workspace flatpak distrobox podman` | compound 0; one package absent |
| `stat` plus `findmnt -T` for home/config/local/systemd/Flatpak/Docker/run-user paths | 0 |
| bounded `find /home/deck/.config/systemd/user -maxdepth 3` | 0 |
| `systemctl --user list-unit-files --no-pager` | 1; user bus denied |
| `flatpak list --columns=application,ref,version,branch,installation` | 0 |
| `flatpak remotes --show-details` | 0 |
| `test -w` for selected existing paths | 0 |
| bounded `find /home/deck/.local/share/flatpak -maxdepth 3` | 0 |

## 9. External source consultation

No host command or download was used. The official Microsoft exFAT File System Specification was read through the web retrieval tool on 2026-07-14 and cited in AUD-0001:

`https://learn.microsoft.com/en-us/windows/win32/fileio/exfat-specification`

## 10. Record creation and validation commands

The three authorized Markdown records were created and amended through workspace patch operations:

```text
records/audits/AUD-0001-initial-steam-deck-baseline.md
records/executions/RUN-0003-perform-initial-steam-deck-baseline-audit.md
records/validations/VAL-0003-initial-steam-deck-baseline-audit-validation.md
```

Final validation and presentation commands are recorded in VAL-0003 after execution.

Completion time was recorded with `date --iso-8601=seconds`; exit `0`.

## Warnings, permission failures, and unexpected behavior

1. `plasmashell --version` aborted with exit 134. The OS may have attempted an automatic coredump; the audit did not intentionally create one.
2. System and user D-Bus access was blocked, affecting hostnamectl, loginctl, systemctl, NetworkManager, firewalld, and resolved queries.
3. Netlink was blocked, affecting interface/address/route/socket/nft queries.
4. `sudo -n true` was blocked by `no_new_privs` and unexpected sandbox ownership mapping. Sudo capability remains unknown; no password was requested.
5. Graphics display/GPU enumeration failed in the restricted environment.
6. The audit VFS presents many host paths as read-only while underlying filesystem options can report read-write.
7. `podman --version` attempted to create `/run/user/1000/libpod`; creation failed with a read-only error. No Podman state command was attempted afterward.
8. Full firewall state and host port usage were inaccessible.
9. No permission failure occurred in the bounded asset search roots.

## Commands deliberately skipped

- All package installation/update/removal commands.
- All service start/stop/enable/disable commands.
- Docker commands, because Docker was absent.
- Podman info/list/image/container/network/volume commands, to avoid initializing state.
- Full-disk or root-owned asset searches.
- Privilege escalation and password prompts.
- Stress tests, benchmarks, build commands, and runtime launches.
- Filesystem write, delete, rename, symlink, socket, lock, fsync, mount, remount, or repair tests.
- Git initialization, clone, fetch, pull, or remote operations.
- Database, firewall, port-forward, source, client, or container changes.
- AUD-0002 and all implementation work.

## Modification boundary confirmation

Only read-only system inspection and creation/amendment of the three authorized Markdown records occurred. No package, service, container, image, repository, source tree, database, firewall rule, network configuration, mount, permission, ownership, runtime directory outside the project, AzerothCore asset, game-client asset, Git repository, or later audit was intentionally created or modified. The project remains **S0 — Proposed**.

## Final presentation and revalidation commands

Pre-presentation line counts were obtained with:

```bash
wc -l \
  /run/media/deck/blAIneXPLAT/azerothcore/records/audits/AUD-0001-initial-steam-deck-baseline.md \
  /run/media/deck/blAIneXPLAT/azerothcore/records/executions/RUN-0003-perform-initial-steam-deck-baseline-audit.md \
  /run/media/deck/blAIneXPLAT/azerothcore/records/validations/VAL-0003-initial-steam-deck-baseline-audit-validation.md
```

Exit `0`; counts before this section was appended were AUD `614`, RUN `300`, VAL `95`.

The completed records are printed without truncation using these exact ranges:

```bash
printf '%s\n' \
  records/audits/AUD-0001-initial-steam-deck-baseline.md \
  records/executions/RUN-0003-perform-initial-steam-deck-baseline-audit.md \
  records/validations/VAL-0003-initial-steam-deck-baseline-audit-validation.md
sed -n '1,220p'   "$root/records/audits/AUD-0001-initial-steam-deck-baseline.md"
sed -n '221,440p' "$root/records/audits/AUD-0001-initial-steam-deck-baseline.md"
sed -n '441,660p' "$root/records/audits/AUD-0001-initial-steam-deck-baseline.md"
sed -n '1,220p'   "$root/records/executions/RUN-0003-perform-initial-steam-deck-baseline-audit.md"
sed -n '221,440p' "$root/records/executions/RUN-0003-perform-initial-steam-deck-baseline-audit.md"
sed -n '1,240p'   "$root/records/validations/VAL-0003-initial-steam-deck-baseline-audit-validation.md"
```

The final explicit validation repeats the path, section, lifecycle, non-installation, non-operational, architecture, inventory, and Git checks described in VAL-0003 and prints its exit status.
