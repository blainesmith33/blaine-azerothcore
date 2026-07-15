# Protocol Versioning

- Status: NOT_IMPLEMENTED
- Current protocol: NOT_YET_SELECTED

Every addon/module/control-plane communication contract shall have an explicit protocol name and version. Module, addon, control definition, preset, migration, and package versions remain independent.

Compatibility negotiation shall declare supported versions and fail safely when protected operations are incompatible. Moving labels are not validated pins. A protocol change shall record schemas, compatibility range, migration impact, replay behavior, release evidence, and rollback or degradation behavior.

