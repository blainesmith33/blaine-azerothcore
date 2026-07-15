# Command Contexts

- Status: NOT_IMPLEMENTED

The operation model keeps four visible channels:

- realm command channel: governed game-server operations;
- container command channel: governed service-runtime operations;
- database command channel: structured data administration;
- host command channel: bounded non-root host operations.

Each context has separate adapters, permissions, targets, previews, risk analysis, results, validation, and rollback evidence. Context selection does not authorize execution. Cross-context sequences require each operation to pass its own checks. Raw access is disabled by default.

