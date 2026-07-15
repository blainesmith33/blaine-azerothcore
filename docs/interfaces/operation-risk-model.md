# Operation Risk Model

- Status: NOT_IMPLEMENTED

| Label | Meaning |
|---|---|
| READ | Intended to observe state without changing it. |
| CHANGE | Intended to make a bounded, reversible state change. |
| DISRUPTIVE | May interrupt availability or active work. |
| DESTRUCTIVE | May erase, irreversibly replace, or invalidate state. |
| PRIVILEGED | Crosses an elevated authorization or trust boundary. |

Labels may be combined. They support review, routing, preview, backup, approval, and validation policy; they are not sufficient authorization by themselves. Actual authorization also requires operator identity, target, compatibility, current state, permissions, and explicit policy.

