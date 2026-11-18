---
name: automate-me
description: "Use for 'automate me', 'create/update/refresh my -mode skill', 'turn/capture my preferences or working style into a skill', or wanting agents to follow how the user works. Drafts or revises a personal -mode skill, optionally pulling fresh evidence from recent conversations."
---

# Automate me

A guided flow for turning the user's working conventions into a skill agents will follow. The output is one `-mode` skill tailored to them (e.g. `betinho-mode`, `priya-mode`).

This skill orchestrates three steps: an inline mining pass (step 1), authoring via `skill_manage`, and the **unslop** skill (prose discipline). It sequences them. It doesn't replace them.

## Flow

### 0. Check for an existing skill

Look for an existing skill named `<handle>-mode` in the skill library. If one exists, confirm intent (unless the user already said "update my skill" or similar):

- Update the existing skill (default for repeat runs)
- Start fresh (rare, ask why before doing it)

Update mode changes the rest of the flow:
- Step 1 mines only history since the skill was last edited.
- Step 2 asks what's changed or missing, not what to capture from zero.
- Step 4 edits the existing file in place. Preserve sections the user hasn't contradicted. Revise ones with new evidence. Add new sections only for genuinely new rules.

### 1. Mine their history

Search this bot's own past conversations with the user for recurring patterns. For a large history, split it into slices (e.g. the last 2-4 weeks, in 2-3 chunks) and have a subagent mine each slice, each returning a short structured list of patterns it saw with evidence pointers. Default signals worth hunting:

- Response preferences (length, tone, format, "dumb it down" corrections)
- Delegation habits (which teammates, which tools, parallelism)
- Verification posture (what "done" means, tests vs live checks, reviewers)
- Code and prose discipline (style, principles cited, lint/format tools)
- Process conventions (commits, PRs, review/merge tooling)
- Meta preferences (fixing skills mid-task, proposing new ones)

Cross-check across slices before elevating a signal. Patterns seen in 2+ slices are high-confidence. Lone signals are weak and usually get dropped.

### 2. Ask the user directly

Mining misses intent that hasn't come up yet. Ask one question at a time with a short list of concrete options. Start broad ("Which areas matter most?"), then follow up on selected areas with specific options. One free-form question at the end catches anything the options missed. Don't dump many questions at once.

### 3. Cluster findings

Group the combined signals into sections. Common ones (use only what applies):

- **Response style**: length, tone, format.
- **Autonomy**: how much to do without asking, which tools to use freely.
- **Understand first**: which skills to reach for when scoping or investigating a change.
- **Delegation**: default teammate, parallelism, task-to-teammate routing.
- **Prose / code discipline**: principles, lint tools, style guides.
- **Review and verify**: repro posture, verification skills, live-testing habits.
- **Process**: commits, PRs, review/merge tooling.
- **Skills**: skill-authoring habits, fix-the-skill-first, proposing new skills.

The **poteto-mode** skill shows the shape. Read it for granularity. Don't copy its content. The user's rules are not the same as poteto-mode's.

### 4. Draft the skill

Author the skill directly (create or update).

- Name: `<handle>-mode`, where handle is the user's first name or chosen identifier.
- `description`: trigger on their name + "work in their style" + explicit phrases like "<handle> mode", not on generic keywords like "write code" or "review PR".
- Keep `description` as one line.

### 5. Iterate on prose

Apply the **unslop** skill to every line.

Show the draft to the user and take feedback. Expect multiple iterations. Cut ruthlessly. A mode skill is not a manual.

### 6. Land it

Save it directly. There's no git worktree or PR step here, since OpenMausBot skills aren't stored in a git repo.

## Guardrails

- **Don't overfit to one conversation.** A preference stated once and contradicted another time is noise. Require multiple instances before codifying it.
- **Don't be clever.** Restating other skills' contents, inventing metaphors, or writing "poetic" prose for an agent reader is cost without benefit. Keep it operational.
- **Reference, don't inline.** Other skills the user relies on should appear as named references, not pasted excerpts.
- **Keep sections minimal.** Only add a section if the user has a specific, non-default rule there. "Communicate clearly" is not a section. "Short paragraphs. Tables when comparing options. Bullets only when items are genuinely parallel." is.
- **Name conventions generic.** Use "the user" or "the human" in imperatives, not the author's first name.
- **Don't force symmetry.** If a user has no process rules worth writing down, skip the Process section entirely.

## Evaluation

A `-mode` skill is subjective output. Vibe-check with the user: does it read like them? Did it miss anything? Then ship.

## When not to use

- User wants a task-specific skill (not working conventions): create it directly, no mining required.
- User wants to capture one narrow workflow (e.g. "how I write commit messages"). That's a regular skill, not a mode skill.
