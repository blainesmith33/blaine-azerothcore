# Governed Upstream Source Checkouts

Lifecycle status: **S0 — Proposed**

Repositories beneath this directory are independently cloned and governed. Their working trees and Git databases are not committed to the `blaine-azerothcore` governance repository. Exact source identity is instead preserved through tracked source manifests and validation records.

The clean baseline must not modify an official upstream checkout. Later custom work must use separately governed branches, modules, or repositories so the pinned upstream baseline remains reproducible.

The official checkout currently expected beneath this directory is `SRC-ACORE-WOTLK-001` at `linux/upstream/azerothcore-wotlk/`.
