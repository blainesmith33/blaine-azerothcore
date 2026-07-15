# Module Integration Promotion Procedure

Status: **Authoritative procedure; not yet executed**  
Current lifecycle status: **S0 — Proposed**

## Promotion sequence

The following order is mandatory for promotion toward the integrated Blaine realm:

1. Validate the module individually on its reference baseline.
2. Record provenance and licensing.
3. Freeze the desired behavior as testable requirements.
4. Select a port, adaptation, replacement, or independent implementation strategy.
5. Build against an exact candidate integration baseline.
6. Add one feature or module at a time with an updated combined manifest.
7. Run database migration, ordering, ownership, backup, and downgrade checks.
8. Run combined compile validation, including clean and repeat builds.
9. Run startup and shutdown validation for affected services and realms.
10. Run gameplay acceptance tests for the promoted behavior.
11. Run symbol, hook, SQL, configuration, port, database, client, conflict, security, stability, and regression tests.
12. Test module removal, data consequences, and rollback to the prior validated manifest.
13. Pin all exact core, module, dependency, database, configuration, toolchain, and client versions and commits.
14. Promote only after linked RUN/VAL evidence and explicit human approval establish that the acceptance criteria are met.

## Gate rules

- A successful individual build, runtime, or gameplay test does not approve integrated use.
- A failed or partially successful step remains in the execution or incident record.
- Promotion is platform-specific; Linux evidence cannot promote Windows and vice versa.
- Any exception requires a decision record before promotion.
- Promotion updates the instance manifest, compatibility matrix, provenance inventory, backups, rollback plan, and lifecycle evidence together.

## Promotion states

Suggested governed states are `concept`, `research`, `active-development`, `integration-candidate`, and `released`. Directory placement does not itself prove a state; the controlling record and evidence do.
