---
name: poteto-playbooks-ops
description: "Reference bundle 3 of 3 for the poteto-mode skill's Playbooks section. Holds: Runtime forensics, Session pickup, Shipping, Trace forensics, Visual parity, Worktree cleanup. Load this when poteto-mode routes to one of those."
---

# Poteto playbooks — ops (3 of 3)

Each reference file below is one playbook, ported from the pstack plugin's `playbooks/<title>.md`. Porting gap, stated once here: references to Cursor cloud agents, Cursor's `/loop` command, Graphite (`gt`), `cursor-team-kit`'s control/deslop skills, and CDP (Chrome DevTools Protocol) live-process instrumentation have no equivalent in this account. Where a playbook calls for one of those, substitute direct delegation, a scheduled automation or continuing across turns, `gh` (already the stated default), a manual verification/slop pass, and whatever profiling/debugging access actually exists on the target surface, respectively. The actual engineering judgment in each file is otherwise unchanged.

- `references/runtime-forensics.md`
- `references/session-pickup.md`
- `references/shipping.md`
- `references/trace-forensics.md`
- `references/visual-parity.md`
- `references/worktree-cleanup.md`