# Client Addon and Server Module Contract

- Project lifecycle: S0 — Proposed
- Implementation status: NOT_IMPLEMENTED
- Protocol status: NOT_YET_VALIDATED

## Trust model

The server is authoritative over protected gameplay state. Addon messages are untrusted input and do not prove identity, permission, target validity, or intent. An addon must not perform or depend on undocumented direct database manipulation.

## Required contract properties

A future contract must define:

- an explicit message and protocol version;
- request and response schemas;
- identity and permission validation;
- input, range, target, compatibility, and state validation;
- replay and duplicate-request handling;
- structured failure reporting;
- compatibility negotiation;
- degradation behavior when the server component is absent;
- correlation identifiers and auditable outcomes.

## Compatibility behavior

Unsupported or incompatible protocol versions fail closed for protected operations. The addon must remain intelligible and degrade safely when its companion module is missing, disabled, unhealthy, or incompatible. No message format, hook, opcode, addon channel, or server API has been selected or validated.

