# ADR-0012: Public Game Access and Private Management Network Boundaries

- Status: Accepted at S0 — Proposed
- Date: 2026-07-14
- Scope: Future network exposure and ingress trust boundaries
- Related decisions: `ADR-0002`, `ADR-0007`, `ADR-0009`, `ADR-0010`, `ADR-0011`
- Related architecture: `docs/architecture/internet-ingress-and-network-trust-boundary-architecture.md`

## Context

The project is intended to support authorized players outside the local network, but existing records cover only rootless Docker validation and host-local listener behavior. They do not establish inbound internet capability or select a deployment ingress. Present-tense documents also described the initial Realm Control profile as local-only without clearly distinguishing that initial boundary from a possible future secured remote-management capability.

The unresolved `pasta` result concerns the listener shape observed during a loopback-requested rootless Docker validation. It is important host-local evidence, but it cannot determine public addressing, router or ISP conditions, an ingress gateway, or internet-origin reachability.

## Decision

Accept **public gameplay with separately governed private and authenticated management boundaries, while deferring the final ingress mechanism pending connectivity and threat-model evidence**.

Specifically:

1. Remote internet gameplay access is a project requirement.
2. Public gameplay and private management are separate trust domains.
3. Only explicitly classified services may receive ingress, with exactly one approved exposure classification per deployment profile.
4. Public gameplay exposure does not authorize database, Docker, host, backup, internal-service, or unrestricted management exposure.
5. blAIne Realm Control may eventually support remote access only through authenticated, encrypted, brokered, authorized, logged, and auditable mechanisms.
6. The Docker socket must never be exposed directly to a browser or the public internet.
7. The existing `pasta` loopback-listener issue remains unresolved for host-local services.
8. That unresolved host-local issue does not decide the final public-ingress design.
9. No ingress technology is selected by this ADR.
10. No current network or runtime change is authorized.
11. The lifecycle remains `S0 — Proposed`.

The provisional classes are `NET-HOST-LOCAL`, `NET-PRIVATE-INTERNAL`, `NET-LAN-RESTRICTED`, `NET-REMOTE-AUTHENTICATED`, `NET-PUBLIC-GAME`, `NET-PUBLIC-WEB`, and `NET-PROHIBITED-DIRECT`.

Exact public gameplay ports are deferred. They must be derived from pinned AzerothCore configuration and approved deployment manifests.

## Rationale

Authorized remote gameplay is a functional requirement, while management authority and sensitive internal dependencies require stronger isolation. Separating planes makes availability and isolation independently testable, prevents a player-facing service from implicitly broadening infrastructure access, and avoids selecting an ingress method before the host's actual connectivity and threat conditions are known.

## Considered alternatives

| Alternative | Disposition |
|---|---|
| Permanently local-only deployment | Rejected because it cannot satisfy required authorized internet gameplay access. |
| Make all service ports directly public | Rejected because it collapses trust domains and would expose management, databases, runtime control, or other sensitive services. |
| Public gameplay with private management separation | Accepted, with the ingress mechanism deferred pending evidence. |
| VPN-only access for all users | Conditional future candidate, but not selected; it may impose player onboarding and client constraints. |
| Relay or gateway-based public access | Conditional future candidate, but not selected; cost, latency, reliability, control, and abuse handling require evidence. |
| Direct router-forwarded public access | Conditional future candidate, but not selected; feasibility and risk depend on verified addressing, router, ISP, firewall, and threat-model conditions. |

## Consequences

- The project must later inventory and classify every networked service per deployment profile.
- Public gameplay availability and private-service non-exposure require independent evidence.
- Host-local baseline validation precedes inbound-connectivity characterization and ingress selection.
- Exact ports, bind addresses, firewall rules, address publication, certificates, gateway components, monitoring, and emergency shutdown remain later decisions.
- Remote management cannot be introduced as an incidental extension of player ingress.

## Security boundaries

Direct public exposure is prohibited for Docker and container-runtime sockets, raw databases and unrestricted database administration, host root interfaces, unbrokered shells, backup interfaces, secret and credential stores, unrestricted Realm Control command execution, internal brokers, and unauthenticated sensitive metrics.

Public gameplay conveys no management authority. Future management actions must traverse an authenticated and encrypted ingress layer and a constrained broker with explicit authorization, validation, logging, audit, revocation, and rollback behavior.

## Deferred decisions

- Actual inbound IPv4 and IPv6 conditions, address stability, carrier-grade NAT, router control, and ISP restrictions.
- Final ingress mechanism and any provider, gateway, relay, tunnel, overlay, proxy, or direct-forwarding design.
- Exact public gameplay services, ports, protocols, binds, firewall policy, and abuse controls.
- Domain, certificate, DDoS, monitoring, uptime, cost, bandwidth, latency, geographic, recovery, and failover requirements.
- Remote Realm Control threat model, identity system, authorization model, and management ingress implementation.

## Validation requirements

Later operations must independently validate host loopback, same-host LAN-address, independent same-LAN-device, internet-origin gameplay, internet-origin management authentication or rejection, public-port enumeration, private-service non-exposure, authorization, TLS where applicable, logging, persistence, revocation, and emergency shutdown behavior.

Successful public gameplay does not prove private-service isolation, and private-service isolation does not prove public gameplay availability.

## Supersession and relationships

This ADR complements ADR-0002's multi-realm design and ADR-0009's project boundary. It clarifies ADR-0007: the initial Realm Control implementation remains host-local and read-only, while a future remote profile may be considered only under the secured management boundary defined here. It does not retroactively alter historical execution evidence.

ADR-0010 and ADR-0011 remain authoritative for the rootless Docker runtime. Their loopback-only validation and unresolved `pasta` listener-shape evidence apply to host-local tests and do not select future public ingress. This ADR supersedes no Docker storage, security, persistence, or runtime decision.

## Explicit non-authorization

This decision authorizes documentation and governance records only. It authorizes no port exposure, listener, firewall rule, router setting, Docker change, network-driver change, VPN, tunnel, DNS, proxy, gateway, relay, cloud service, certificate, connectivity test, or runtime action. No inbound internet capability is assumed. The project lifecycle remains **S0 — Proposed**.
