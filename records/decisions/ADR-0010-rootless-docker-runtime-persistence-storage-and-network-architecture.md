# ADR-0010: Rootless Docker Runtime, Persistence, Storage, and Network Architecture

- Status: Accepted at S0 — Proposed
- Date: 2026-07-14
- Scope: First functional Docker runtime baseline for the current Steam Deck host
- Related evidence: `AUD-0003`, `RUN-0007`, `VAL-0007`

## Context

The project needs a Docker-compatible runtime architecture before any Docker installation or AzerothCore acquisition is authorized. The current host is SteamOS, the operating-system image is kept in its normal read-only mode, `/home/deck` is persistent internal ext4 storage, and project governance records are held at `/run/media/deck/blAIneXPLAT/azerothcore/`.

This decision is deliberately limited to the current Steam Deck baseline. It authorizes an architecture and a later, separately governed functional milestone; it does not authorize installation, downloads, service transitions, image pulls, AzerothCore acquisition, or lifecycle advancement.

## Audit evidence

`AUD-0003` and its passing independent validation in `VAL-0007` establish the following host facts:

- The `deck` user is UID/GID 1000 and has subordinate UID and GID ranges `deck:100000:65536`.
- Unprivileged user namespaces and automatic subordinate-ID mapping were demonstrated by immediate `unshare` tests.
- The systemd user manager is running in the active session; `/run/user/1000` and the user bus are available. `Linger=no`.
- Cgroup v2 is mounted. CPU, memory, and PID controllers are available and delegated to `user@1000.service`; enforcement through Docker has not been tested.
- `/home/deck` is persistent, writable internal ext4 storage with ample capacity. The prospective data directory `/home/deck/.local/share/docker/` does not exist.
- Rootless `overlay2` and `fuse-overlayfs` are plausible but unproven; btrfs is not relevant to the selected ext4 data root; `vfs` is an undesirable fallback.
- Docker Engine, Docker CLI, Docker Compose, RootlessKit, containerd, runc, and the rootless setup tools are absent.
- `slirp4netns` and `vpnkit` are absent. `pasta` exists, but its compatibility with a future pinned Docker/RootlessKit build is unvalidated.
- No audited conflict existed on the planned AzerothCore ports. LAN publication behavior was not tested.
- The current boot retains a serious historical USB/UAS and ext4 error warning for the staging drive. No later audit may treat that problem as resolved without positive evidence.

The prerequisite status is therefore sufficient to choose a conditional architecture, not to claim an operational runtime.

## Current official-source evidence

Official text and release metadata were inspected read-only on 2026-07-14. No installer, archive, binary, repository, image, or other executable content was retrieved.

| Source | Release or behavior evidence | Architectural use |
|---|---|---|
| Docker rootless mode, `https://docs.docker.com/engine/security/rootless/` | Rootless mode runs daemon and containers as a non-root user in a user namespace; the documented prerequisites include `newuidmap`, `newgidmap`, and at least 65,536 subordinate IDs. The user-level setup creates a systemd user service and rootless CLI context. | Supports the rootless `deck`-user selection and later prerequisite enforcement. |
| Docker rootless tips, `https://docs.docker.com/engine/security/rootless/tips/` | Documents `~/.config/systemd/user/docker.service`, user-service control, lingering for boot persistence, `$XDG_RUNTIME_DIR/docker.sock`, default data under `~/.local/share/docker`, daemon config under `~/.config/docker`, and distinct CLI config under `~/.docker`. | Establishes approved user layout and the login-session/lingering boundary. |
| Docker rootless troubleshooting, `https://docs.docker.com/engine/security/rootless/troubleshoot/` | Current RootlessKit defaults are conditional: `gvisor-tap-vsock` with `builtin` when `slirp4netns` is absent, and `slirp4netns` with `builtin` when it is installed. `vpnkit` is legacy. `pasta` is documented as experimental and requires compatible versions. | Establishes the detected-default network policy and prohibition on forcing pasta or slirp4netns. |
| Docker Engine v29 release notes, `https://docs.docker.com/engine/release-notes/29/` | `29.6.1`, dated 2026-06-26, is the newest listed v29 release. | Stable Engine candidate: `29.6.1`. |
| Docker stable x86_64 index, `https://download.docker.com/linux/static/stable/x86_64/` | Stable index lists both `docker-29.6.1.tgz` and `docker-rootless-extras-29.6.1.tgz`. No prerelease, beta, RC, or test marker is present. | Confirms current stable static/rootless candidate availability; no archive was downloaded. |
| Official `docker/compose` release, `https://github.com/docker/compose/releases/tag/v5.3.1` | `v5.3.1` is marked Latest, dated 2026-07-07, at verified signed tag/commit `f32009d`. It is not marked prerelease, beta, RC, or test-channel. | Stable Compose plugin candidate: `v5.3.1`. |
| Official `docker/compose` repository, `https://github.com/docker/compose` | Documents the Linux user plugin path under `$HOME/.docker/cli-plugins`. | Supports the approved Compose plugin location. |
| AzerothCore Docker guide, `https://www.azerothcore.org/wiki/install-with-docker` | Lists Git, Docker, and the Docker Compose plugin as prerequisites and uses the main-repository Compose workflow. It cautions that some configuration may appear non-idiomatic and recommends production administrators understand Docker. | Confirms workflow compatibility while preserving the operational caution. |
| AzerothCore FAQ, `https://www.azerothcore.org/wiki/faq` | Explicitly classifies Docker among supported platforms. | Current official classification is supported, not unsupported; support does not prove this host baseline. |
| AzerothCore getting started, `https://www.azerothcore.org/wiki/getting-started` | Describes Docker as the simplified setup path where Docker is installed. | Supports later compatibility evaluation, not acquisition now. |

The Docker sources agree materially on the stable user-level rootless path. The AzerothCore sources classify Docker as supported while retaining operational cautions. No newer prerelease was substituted for the stable candidates.

## Decision

### Runtime type

The first functional baseline will use a rootless Docker Engine operated by the `deck` user.

The initial baseline rejects:

- a rootful Docker daemon;
- Docker socket access by an unrestricted browser application;
- Docker API exposure over TCP;
- Docker-in-Docker;
- a Distrobox-hosted Docker daemon;
- permanent Podman substitution; and
- installation into the immutable SteamOS operating-system image.

This selection is host-specific and may be revised only by a later evidence-based ADR.

### Installation method and version pinning requirement

A later, separately authorized operation will use Docker's official rootless, user-level installation mechanism for a host without a supported Docker package repository. That operation must:

1. Download the official installer as a file and never pipe remote content directly into a shell.
2. Record the installer's SHA-256 and installer-declared source commit.
3. Compare it with its official source representation where practical and inspect it before execution.
4. Re-resolve and explicitly pin the exact stable Docker Engine and Compose plugin versions at execution time. The current architecture candidates are Engine `29.6.1` and Compose `v5.3.1`; a later change in stable metadata must be recorded, reviewed, and pinned rather than silently followed.
5. Download the Docker Engine/rootless archives and Compose binary separately, record SHA-256 values before installation, and reconcile the inspected installer with those exact pinned artifacts. If the official mechanism would fetch an unpinned or unexpected artifact, stop.
6. Reject test-channel, beta, RC, prerelease, or unexpectedly unsigned/unverifiable artifacts.
7. Install only beneath the persistent `deck` home directory.
8. Keep SteamOS read-only mode enabled, make no pacman transaction or initialization, and place no project-managed Docker binary in `/usr` or `/usr/local`.

This ADR records candidate versions; it does not approve their artifacts before the later operation verifies provenance and checksums.

### User-level locations

The official rootless layout is approved unless later installation evidence requires an explicit, reviewed deviation:

| Purpose | Approved location |
|---|---|
| Docker executables | `/home/deck/bin/` |
| Docker daemon configuration | `/home/deck/.config/docker/` |
| Docker CLI configuration and plugins | `/home/deck/.docker/` |
| Docker Compose CLI plugin | `/home/deck/.docker/cli-plugins/docker-compose` |
| Docker data root | `/home/deck/.local/share/docker/` |
| Rootless Docker socket | `/run/user/1000/docker.sock` |
| User systemd unit | `/home/deck/.config/systemd/user/docker.service` |

The daemon configuration directory `/home/deck/.config/docker/` and CLI configuration directory `/home/deck/.docker/` are different and must not be conflated.

## Storage boundary

Internal ext4 storage under `/home/deck` is approved for the Docker data root, container writable layers, image layers, build cache, initial database named volumes, initial operational logs, and temporary runtime state.

Docker's active data root must not be placed on `blAIneXPLAT`, the historically unreliable USB/UAS staging drive, exFAT, a network filesystem, or the immutable root filesystem. The authoritative governance records remain at `/run/media/deck/blAIneXPLAT/azerothcore/`.

Future AzerothCore source, configuration, modules, and original work must remain under governed project paths and applicable Git/version-control rules. No unique original work may exist only on the historically unreliable staging drive.

Database backup architecture is deferred. A future backup must not exist solely inside Docker's active data root.

The later functional test must report the actual storage driver and backing filesystem. `overlay2` is the preferred candidate on the audited internal ext4/user-namespace host, `fuse-overlayfs` is a conditional fallback, btrfs is irrelevant to this placement, and `vfs` is an emergency compatibility fallback with unacceptable normal space/performance cost. No driver is declared functional until Docker proves it.

## Persistence boundary

For the first installation and functional test:

- do not enable user lingering;
- do not modify `loginctl` configuration;
- do not create a root system service;
- start and test Docker only inside the active `deck` user session; and
- treat logout and reboot persistence as unvalidated.

`Linger=no` is not a blocker for an active-session functional test. It is a blocker for unattended startup before login and continued operation after the user's session ends. A later separate operation may authorize `loginctl enable-linger deck` only after Docker is installed and validated and the persistence implications are reviewed.

## Network boundary

The initial Docker validation will use Docker's current supported default rootless network driver selection. It will not force pasta or slirp4netns. Given the audited absence of slirp4netns, current documentation suggests `gvisor-tap-vsock` with the `builtin` port driver, but that is an expectation, not a finding; the actual installed runtime must report its selected network and port drivers.

Docker management remains on the local Unix socket. The Docker API must not be exposed over TCP. Initial validation may use only disposable test containers, and its first published test port must bind to loopback only. It must not change firewalld, expose AzerothCore ports, or assume LAN reachability.

The later Docker validation must record:

- actual rootless network driver and port driver;
- outbound DNS and HTTPS connectivity;
- loopback-published-port behavior;
- container-to-host isolation behavior;
- Docker rootless security status;
- actual storage driver; and
- cgroup version and driver.

LAN exposure of AzerothCore ports requires a separate governed network operation after the clean baseline works locally.

## Resource governance

`AUD-0003` shows cgroup v2 and CPU, memory, and PID controller delegation to the user manager. This makes limits plausible; actual Docker enforcement remains unvalidated. The later functional test must use a harmless disposable container to verify bounded CPU, memory, and PID limits. No stress test is authorized.

## Security boundary

The initial baseline requires:

- a rootless daemon;
- local Unix socket access only;
- no browser access to the Docker socket;
- no TCP Docker API;
- no host root shell;
- no privileged test container;
- no host network mode;
- no mount of `/`, `/etc`, `/home`, `/run`, or the Docker socket into a test container;
- no credentials in project records;
- no plaintext database passwords in governance records; and
- no AzerothCore administrative credentials yet.

Rootless Docker reduces host privilege but remains powerful over the `deck` user's files and processes. Socket access must therefore remain restricted to trusted local user processes.

## Functional milestone

The exact next milestone is **Rootless Docker Functional Runtime**.

Success requires all of the following in a separately authorized operation:

- pinned Docker Engine installed under `/home/deck`;
- pinned Docker Compose plugin installed for `deck`;
- Docker user service starts successfully;
- rootless context is active;
- `docker version`, `docker info`, and `docker compose version` succeed;
- security options explicitly report rootless operation;
- data root is internal ext4 storage;
- a disposable official test image can be pulled;
- a disposable container runs and exits successfully;
- a disposable loopback-only HTTP container can be reached from the host;
- no LAN exposure is created;
- CPU, memory, and PID limits are accepted and observed without a stress test;
- test containers and images are removed afterward unless explicitly retained as evidence;
- Docker remains installed;
- no AzerothCore content is acquired; and
- no new relevant storage errors occur.

The project remains `S0 — Proposed` after that Docker-only validation because AzerothCore itself will still not exist.

## Rollback requirement

The later installation operation must capture a pre-install inventory and provide an exact rollback limited to artifacts that operation creates. It must distinguish:

- stopping and disabling the user Docker service;
- removing the rootless Docker CLI context;
- removing installed user-level Docker binaries;
- removing the Compose plugin;
- removing created user service files;
- removing or preserving Docker data as a separate choice;
- preserving validation evidence; and
- avoiding deletion of every pre-existing user file or directory.

Rollback must not be executed unless required by authorized failure handling or separately authorized.

## Alternatives considered

| Alternative | Disposition and reason |
|---|---|
| Rootful Docker daemon | Rejected for the initial baseline: broader host-root authority, OS integration, and immutable-image persistence risk. |
| Docker installed in the SteamOS image | Rejected: conflicts with the read-only/update-persistence boundary and would require OS/package changes. |
| Static Docker binaries without the official rootless user-level setup mechanism | Rejected as the architecture by itself: payload provenance is relevant, but service/context/rootless setup must follow the inspected official mechanism. |
| Docker-in-Docker | Rejected: adds privilege, nested storage/network/cgroup complexity, and does not provide the selected host runtime. |
| Distrobox-hosted Docker daemon | Rejected: adds an environment layer and awkward service, storage, network, and Compose ownership boundaries. |
| Permanent Podman substitution | Rejected: user intent is Docker and AzerothCore's main workflow is Docker Compose. Podman will not be silently substituted. |
| Forced pasta | Rejected for the initial baseline: official Docker documentation currently labels it experimental. |
| Forced slirp4netns | Rejected for the initial baseline: it is absent and the detected supported default should be observed first. |
| External-drive Docker data root | Rejected: governance exFAT and the staging drive's unresolved USB/UAS reliability warning are inappropriate for active runtime data. |

## Consequences

The architecture minimizes OS-image changes and host-root authority and places mutable runtime state on persistent internal storage. It preserves compatibility with Docker Compose and AzerothCore's official main-repository workflow. It also makes Docker availability dependent on the logged-in `deck` user session until lingering is separately reviewed.

No operational claim follows from this ADR. Network, storage-driver, cgroup-limit, service, context, and Compose behavior remain to be proven with pinned artifacts.

## Deferred decisions

- Whether and when to enable lingering for unattended boot/logout persistence.
- Exact artifact hashes, installer source commit, and final stable pins at installation time.
- Actual rootless network and port drivers selected by the pinned runtime.
- Actual storage driver and image-store behavior.
- LAN exposure, AzerothCore port bindings, firewall policy, and remote-client reachability.
- AzerothCore source acquisition, Compose configuration, database initialization, modules, addons, and client-data handling.
- Database backup, restore, retention, and off-data-root storage architecture.
- Any revision required by later SteamOS, kernel, Docker, or hardware evidence.

## Lifecycle statement

This ADR is accepted as an S0 architecture record only. No Docker installation or AzerothCore acquisition occurred. The project lifecycle remains **S0 — Proposed**.

