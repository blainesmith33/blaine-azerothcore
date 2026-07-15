# Command Definition Requirements

- Project lifecycle: S0 — Proposed
- Implementation status: NOT_IMPLEMENTED

A command definition is declarative and non-executable until its adapter, compatibility, permission, preview, validation, rollback, and approval evidence is accepted.

Every definition shall identify schema version, command identity, lifecycle, implementation status, target context and types, compatible instances and commits, dependencies, permissions, risk, inputs, preconditions, adapter, operation template, preview, confirmation, backup, validation, rollback, audit fields, provenance, license, and notes.

Operation templates created at S0 shall contain only UNVERIFIED_COMMAND_TEMPLATE. Examples shall be marked EXAMPLE_ONLY, NOT_EXECUTABLE, and COMMAND_SYNTAX_UNVERIFIED. No guessed AzerothCore, SQL, container, or host syntax is permitted.

Risk labels assist routing but never supply authorization. Missing or unresolved compatibility, permission, target-state, or validation data shall prevent execution.

