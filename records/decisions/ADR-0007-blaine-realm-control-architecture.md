# ADR-0007: blAIne Realm Control Architecture

- Status: Accepted at S0 — Proposed
- Date: 2026-07-14
- Scope: AzerothCore development project
- Component maturity: RC-S0 — Proposed architecture

## Context

blAIne is the broader umbrella orchestration and AI-systems-development identity. The AzerothCore development project is subordinate to blAIne, and blAIne Realm Control is a provisional, subordinate component of that project. This decision does not redefine blAIne.

Multiple planned realms require a control plane that can observe a realm even when its worldserver is stopped or unhealthy. Embedding the control plane inside a worldserver container would create shared failure modes and prevent that observation.

## Decision

blAIne Realm Control will be a separate control-plane component and will not be embedded inside a worldserver container. Initial operation is local-only, remote internet exposure is not approved, and the first implementation stage is read-only.

A governed backend command broker will mediate every operation. Browser or visual clients will not receive direct unrestricted access to the Docker socket, database root credentials, AzerothCore administrative credentials, unrestricted host root access, or arbitrary filesystem write access.

Command contexts remain visibly distinct:

- realm command channel;
- container command channel;
- database command channel;
- host command channel.

Every submission must identify the operator, session, command context, target realm, target service or database, privilege level, requested operation, input values, execution result, validation result, and rollback information when applicable.

Raw command access may be considered later but is disabled by default. Host-shell access begins as a non-root user. Privilege elevation requires a separate, explicit authorization boundary. Risk labels do not constitute authorization.

## Component maturity stages

- RC-S0 — Proposed architecture
- RC-S1 — Read-only observatory
- RC-S2 — Controlled service operations
- RC-S3 — Governed realm command execution
- RC-S4 — Structured database administration
- RC-S5 — Full-stack feature integration
- RC-S6 — Validated release candidate

These are component maturity stages, not replacements for the governed project lifecycle.

## Consequences

The control plane must report realm failure without depending on the failed realm. Adapters, permissions, previews, audit records, validation, and rollback evidence must remain explicit. No syntax, server hook, protocol, or behavior is validated by this S0 decision.

