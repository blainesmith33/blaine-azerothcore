# Realm Control Requirements

- Project lifecycle: S0 — Proposed
- Implementation status: NOT_IMPLEMENTED

1. blAIne Realm Control shall remain subordinate to the AzerothCore project and the broader blAIne umbrella.
2. The control plane shall be separable from every worldserver and able to observe stopped or unhealthy realms.
3. Initial operation shall use the `NET-HOST-LOCAL` exposure class and shall be read-only.
4. Future remote management may be considered only as a separately approved `NET-REMOTE-AUTHENTICATED` capability using authenticated, encrypted, brokered, authorized, logged, and auditable ingress; no such ingress is currently approved or implemented.
5. A backend broker shall mediate operations through distinct realm, container, database, and host contexts.
6. A browser shall not receive unrestricted infrastructure credentials, privileged runtime sockets, root access, or arbitrary filesystem writes.
7. Every submitted operation shall carry operator, session, target, privilege, inputs, result, validation, and rollback metadata as applicable.
8. Raw command access shall be disabled by default.
9. Host access shall begin non-root; elevation shall cross a separate explicit authorization boundary.
10. Observability, changes, disruption, destruction, and privilege shall have distinct review and validation requirements.
11. Realm Control maturity shall use RC-S0 through RC-S6 without changing the project lifecycle.
12. No command or adapter syntax shall be authoritative until separately executed and validated.
13. Public gameplay access shall remain a separate trust domain and shall not grant management authority.
14. Docker and container-runtime sockets, raw database interfaces, backup interfaces, secret stores, host root interfaces, and unrestricted command execution shall never receive direct public exposure.
