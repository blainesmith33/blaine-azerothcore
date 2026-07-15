# Command Broker and Execution Model

- Project lifecycle: S0 — Proposed
- Implementation status: NOT_IMPLEMENTED
- Command syntax status: UNVERIFIED

Visual controls create a typed intermediate operation request; they do not directly generate or execute a platform command.

```text
Visual control
→ validated operation request
→ compatibility check
→ permission check
→ target-state check
→ generated adapter operation
→ preview
→ authorization
→ execution
→ result capture
→ postcondition validation
→ record creation
```

## Request boundary

An operation request identifies operator, session, context, target realm, target service or database, privilege level, operation, inputs, requested risk, compatibility constraints, validation, and rollback intent. The broker rejects missing, unresolved, incompatible, unauthorized, or stale requests before adapter execution.

## Context adapters

Realm, container, database, and host operations use separate adapters and permissions. A definition selects an adapter interface, never unrestricted execution. The browser does not possess infrastructure credentials or privileged handles.

A generated adapter operation must be previewed when required, explicitly authorized, captured with its result, and followed by postcondition validation. Failed attempts remain evidence.

## Remote-management ingress boundary

Any future remote Realm Control interface must enter through an authenticated and encrypted management gateway and submit constrained requests to this broker. Public gameplay ingress is a separate trust domain and conveys no management authority. The Docker socket, container runtime control socket, raw database interfaces, host interfaces, backup stores, secret stores, and internal message brokers must never be exposed directly to a browser or the public internet.

## Current boundary

No actual AzerothCore command syntax, SQL statement, container command, host command, server hook, or adapter behavior is authoritative or validated in this S0 scaffold. All operation templates remain placeholders.
