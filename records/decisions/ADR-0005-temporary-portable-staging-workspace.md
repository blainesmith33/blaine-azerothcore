# ADR-0005 — Temporary Portable Staging Workspace

Status: Accepted for S0 scaffolding  
Date: 2026-07-14  
Lifecycle: S0 — Proposed

## Context

The permanent Linux-native runtime target is undecided. A separate ext4 drive can support temporary preparation. AUD-0002 confirmed the current host mount is read/write and not write-protected while preserving historical USB/UAS and I/O instability.

## Decision

- Use /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/ as temporary staging.
- Do not designate it as the permanent trusted runtime.
- Preserve /run/media/deck/blAIneXPLAT/azerothcore/ as authoritative.
- Centralize paths.
- Prohibit secrets, acquired source, builds, live runtime state, databases, container storage, client data, and operational services at S0.
- Require checksum-backed migration and post-copy validation.
- Prohibit keeping the only copy of unique data on staging.
- Require later approval of the permanent target.

## Consequences and evidence

Future work must resolve NOT_YET_SELECTED explicitly. Transport risk remains visible before trust-bearing use. Evidence: INC-0001, AUD-0002, RUN-0004, VAL-0004, RUN-0005, and VAL-0005.

