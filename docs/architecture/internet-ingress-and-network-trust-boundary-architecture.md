# Internet Ingress and Network Trust-Boundary Architecture

- Status: Authoritative S0 proposal; not implemented or operational
- Lifecycle: S0 — Proposed
- Decision: ADR-0012

## Internet accessibility requirement

The project is intended to support authorized players connecting from outside the local network. Public internet accessibility is a required future deployment capability. It must be intentional, governed, authenticated where appropriate, encrypted where appropriate, and independently validated.

This requirement does not make every project service public. No public accessibility, inbound-hosting capability, ingress path, public address, router behavior, or external reachability has been implemented or validated.

## Public gameplay plane

The public gameplay plane contains only the minimum services required by authorized remote players: realm authentication traffic, worldserver gameplay traffic, and future explicitly approved public gameplay endpoints.

Exact ports are not approved here. They must be derived from pinned AzerothCore configuration and recorded in deployment manifests. Before exposure, every public gameplay service must record:

- explicit purpose and owning realm or service;
- protocol, configured bind address, and ingress path;
- firewall policy and authentication assumptions;
- rate and abuse controls where applicable;
- logging and monitoring;
- external validation evidence; and
- rollback procedure.

## Secured remote-management plane

Remote management is a separate trust domain for a future blAIne Realm Control web interface, authenticated administrative APIs, approved operator access, health and observability views, and governed command requests.

Remote management must use an authenticated and encrypted ingress layer. Candidate classes for later research include VPN or private overlay access, an authenticated reverse proxy, an identity-aware access gateway, a reverse tunnel, a relay or gateway, and direct ingress with strong authentication and TLS. This architecture selects none of them. Final selection depends on verified connectivity, threat model, cost, reliability, and operational-control requirements.

Management requests must pass through a constrained command/API broker that enforces authentication, authorization, target and context separation, request validation, logging, auditability, and rollback evidence.

## Private internal service plane

Private internal services are reachable only by explicitly authorized internal components. They may include databases, container-to-container service traffic, internal metrics endpoints, backup services, command brokers, internal Realm Control APIs, service discovery, queues or message infrastructure, and internal administrative endpoints.

Public gameplay exposure does not authorize public reachability for any private internal service.

## Host-local plane

Host-local services normally bind only to loopback or an equivalently restricted local mechanism. This category may include development dashboards, disposable validation listeners, local-only diagnostics, local database administration, development Realm Control instances, and build or orchestration helpers.

The unresolved rootless Docker `pasta` listener-shape result applies primarily to validation of this host-local category. It neither proves public accessibility nor determines the future public-ingress architecture.

## Never-directly-exposed services

The following must never be exposed directly to the public internet:

- the Docker socket or any container-runtime control socket;
- raw database ports or unrestricted database administration;
- host root interfaces or unbrokered shell execution;
- backup storage interfaces;
- secret or credential stores;
- unrestricted Realm Control command execution;
- internal message brokers; and
- unauthenticated metrics containing sensitive information.

Management actions must eventually pass through a constrained, authenticated, authorized, logged, and auditable broker.

## Exposure classification

Every networked service must eventually receive exactly one approved exposure classification per deployment profile.

| Class | Intended audience | Permitted ingress | Authentication | Encryption | Validation requirement | Examples | Prohibited assumptions |
|---|---|---|---|---|---|---|---|
| `NET-HOST-LOCAL` | Processes or operators on the host | Loopback or equivalently restricted local mechanism | Context-dependent; administrative actions still require authorization | Required where sensitive data crosses a process boundary; otherwise profile-specific | Host binding, same-host rejection paths, listener shape, and restart behavior | Diagnostics, disposable listeners, development dashboards | A requested loopback bind proves kernel listener shape or LAN isolation |
| `NET-PRIVATE-INTERNAL` | Explicitly authorized internal components | Private service network only | Mutual service identity or equivalent where applicable | Required for sensitive or cross-host traffic; otherwise threat-model decision | Public non-exposure, service identity, segmentation, and least privilege | Databases, brokers, queues, internal metrics | Container adjacency makes a service safe or public gameplay authorizes access |
| `NET-LAN-RESTRICTED` | Approved devices or operators on a governed local network | Explicit LAN ingress only | Required for management; service-specific for non-management use | Required for sensitive and management traffic | Independent same-LAN device tests and rejection outside the approved LAN scope | Approved local operator or testing endpoint | LAN membership alone is identity or internet reachability is impossible |
| `NET-REMOTE-AUTHENTICATED` | Authorized remote administrators or operators | Authenticated management gateway only | Strong authentication and explicit authorization required | Required | Internet-origin authentication, rejection, authorization, logging, revocation, and private-service isolation | Realm Control, approved administrative APIs | A public gameplay session grants management authority |
| `NET-PUBLIC-GAME` | Authorized remote players | Governed public gameplay ingress | Game/service authentication as defined by pinned configuration | Required where supported and appropriate; limitations must be documented | Independent internet gameplay test, abuse controls, monitoring, rollback, and private-service isolation | Realm authentication and worldserver gameplay traffic | Exact ports, ISP capability, or private-service reachability are implied |
| `NET-PUBLIC-WEB` | Intended public web audience | Governed public web ingress | Service-specific; administrative functions require a different class | TLS required | External TLS, content, authentication, headers, logging, abuse, and isolation tests | Future explicitly public informational endpoints | Public web classification permits administrative or backend access |
| `NET-PROHIBITED-DIRECT` | No direct remote audience | No direct public ingress | Not applicable as an exposure mechanism | Not applicable as an exposure mechanism | Public enumeration and negative reachability proof | Docker sockets, raw databases, host root, backups, secrets | Strong credentials make direct public exposure acceptable |

Classification is profile-specific and does not replace service ownership, protocol, bind, firewall, monitoring, or rollback records.

## Provisional trust zones

```text
Untrusted Internet
        |
        v
Governed Public Ingress
        |
        +--> Public Gameplay Plane
        |
        +--> Authenticated Management Gateway
                    |
                    v
             Command/API Broker
                    |
        +-----------+-----------+
        |                       |
        v                       v
Private Service Plane     Observability Plane
        |
        v
Databases, containers, backups, and internal services
```

This diagram is a logical architecture, not evidence of currently deployed infrastructure.

## Ingress architecture decision inputs

The eventual ingress design depends on evidence not yet collected:

- public IPv4 availability and static versus dynamic addressing;
- carrier-grade NAT and usable inbound IPv6;
- router ownership and configuration access;
- ISP or hotspot restrictions and blocked inbound ports;
- uptime expectations and whether the host is continuously online;
- domain and certificate requirements;
- relay or gateway cost, latency, and bandwidth;
- DDoS and abuse exposure;
- expected player count and geographic distribution; and
- recovery and failover requirements.

No answer to these questions is claimed by this architecture.

## External validation layers

Future ingress validation must treat these as separate evidence layers:

1. Host loopback behavior.
2. Same-host LAN-address behavior.
3. Independent same-LAN-device behavior.
4. Internet-origin gameplay connectivity.
5. Internet-origin management rejection or authentication.
6. Public-port enumeration.
7. Private-service non-exposure.
8. Authentication and authorization enforcement.
9. TLS and certificate validation where applicable.
10. Logging and audit evidence.
11. Restart and persistence behavior.
12. Revocation and emergency shutdown.

Successful public gameplay access does not prove private-service isolation. Private-service isolation does not prove public gameplay availability.

## Current non-implementation statement

This document records requirements and logical boundaries only. It authorizes no port, bind, firewall, router, Docker, VPN, tunnel, DNS, proxy, gateway, relay, or ingress change. The lifecycle remains **S0 — Proposed**.
