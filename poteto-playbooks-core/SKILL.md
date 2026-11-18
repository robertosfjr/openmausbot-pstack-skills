---
name: poteto-playbooks-core
description: "Reference bundle 1 of 3 for the poteto-mode skill's Playbooks section. Holds: Authoring a skill, Autonomous run, Autopilot-full, Autopilot-stack, Babysit, Bug fix, Eval, Feature. Load this when poteto-mode routes to one of those."
---

# Poteto playbooks — core (1 of 3)

Each reference file below is one playbook, ported from the pstack plugin's `playbooks/<title>.md`. These were written for Cursor's own tooling. Porting gap, stated once here rather than edited line by line into every file: references to Cursor cloud agents, Cursor's `/loop` command, Graphite (`gt`), and `cursor-team-kit`'s `control-ui`/`control-cli`/`deslop` skills have no equivalent in this account. Where a playbook calls for one of those, substitute direct delegation (a subagent or a `coordinate_bots` teammate) for a Cursor cloud agent, a scheduled automation or just continuing across turns for `/loop`, `gh` (already the stated default) for Graphite, and a manual verification/slop pass for the missing control/deslop skills. `gh pr` / git commands, file paths, and every piece of actual engineering judgment are unchanged and still apply as written.

- `references/authoring-a-skill.md`
- `references/autonomous-run.md`
- `references/autopilot-full.md`
- `references/autopilot-stack.md`
- `references/babysit.md`
- `references/bug-fix.md`
- `references/eval.md`
- `references/feature.md`