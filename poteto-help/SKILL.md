---
name: poteto-help
description: Guides users through the ported pstack skills, poteto-mode style, and picking the right skill, playbook, or principle for a task. Ask with a question about pstack.
---

# Poteto help

Answer the user's question about the ported pstack skills, hand them a prompt they can send, and name the skill the answer came from. For a help question, don't start the work. The user asked how, and a poteto-mode-style run spends real tokens, so let them send the prompt.

A message that asks for work, such as "use pstack to fix this bug", is not a help question. Read the **poteto-mode** skill, do the work under it.

This file maps questions to the ported skills that hold the answers. Those skills own the details. Read the skill you route to before you quote it, and trust it when it disagrees with this map. The un-ported original lives at `https://github.com/cursor/plugins/blob/main/pstack/` for anyone who wants to compare.

## Find out what they need

Infer the need from the message and the conversation. A named situation, such as "which skill reviews a PR?", goes straight to its section. If the need is still unclear, ask one multiple-choice question with these options, then answer only the section they pick:

- Get set up
- Start a task in poteto-mode style
- Pick a skill for a situation
- Fix a run that went wrong
- Make pstack my own

Check the state that changes the answer, and mention it only when it does:

- No saved reasoning-effort preference means **setup-pstack** hasn't run yet for this user, so every role uses its default effort.
- No verification doc/skill or other app harness in the project means agents have no scripted way to drive the app. Mention **create-verification-skill** when the question is about proving a change works.
- No formal verification skill in the project means no property is checked past what tests reach. Mention **create-formal-verification-skill** when the question is about a race, an invariant, or another case tests rarely hit. When one exists and the code it covers changed since, mention **maintain-formal-verification-skill** if the question touches that code.

## Get set up

This whole plugin — every pstack skill, plus its two subagent roles (Comment Sicko, Poteto Agent) — was ported directly into this workspace's skill library. There's no install step. Start a real task by asking for poteto-mode style, with a goal and a check that can pass or fail (see the recipes below for wording).

If cost is the worry: say where the tokens go (subagents and review panels), and that in this account every subagent runs on Claude (Sonnet 5) — there is no multi-vendor panel to shrink, since Gemini and Grok aren't connected here. Run **setup-pstack** to pick a smaller reasoning-effort budget. Save poteto-mode style for work that needs rigor.

Dropped from the original, with no equivalent here: Cursor's Custom Modes (a mode that silently stays attached across turns without retyping it), the Claude Code plugin adapter, and the separate `cursor-team-kit` plugin's `/deslop`, `control-cli`, `control-ui`. Cursor's `/loop` (an unattended run against a pass/fail check) has no direct equivalent; the closest things here are asking a teammate to keep working via delegation, or a scheduled automation for a recurring check. Cursor's `/create-skill` built-in is this workspace's own skill-management tool, used directly rather than as a skill.

## Start a task in poteto-mode style

Asking for poteto-mode style matches the task to a playbook, puts the playbook's steps on a todo list, and runs the other skills as the steps need them. A good prompt states the goal and how to tell it's done. It doesn't list skills, because a hand-written sequence tends to drop or reorder steps the playbook would keep. Read the prompting notes below before helping word one.

There's no mode that silently stays attached across turns the way Cursor's Custom Mode does — ask for poteto-mode style again on a later message if you want it applied there too. Mid-chat, "new task" makes the style match a fresh playbook.

## Pick a skill

The default answer is **poteto-mode** style, which runs most of the others when its steps need them. Name a skill directly when the user wants more or less of something than the playbook gives. Read the skill before you recommend it, and give one example prompt.

| The user wants to | Skill |
|---|---|
| Do any non-trivial task with rigor | **poteto-mode** |
| Check that the agent understood their goal and the problem before it starts | **in-your-own-words** |
| Get a prompt rewritten deeper, not bigger, before sending it | **sharpen** |
| Know how code works now, or where new code should live | **how** |
| Know why code is shaped this way, or where a number came from | **why** |
| Understand a change or subsystem, explained plainly | **teach** |
| Catch up on their own recent work on a topic | **recall** |
| Catch up on what changed in this chat since their last message | **ketchup** |
| Catch up like ketchup, as a short subtitled video of slides | **ketchup-video** |
| Know what a small diff could break outside itself | **blast-radius** |
| Settle types and module shape before code that crosses a function boundary | **architect** |
| Get several attempts at one brief, merged into the best one | **arena** |
| Run parallel checks over slices, or race workers | **swarm** |
| Have reviewers try to break a diff | **interrogate** |
| Fix a bug test-first when a cheap local test exists | **tdd** |
| Apply TypeScript rules to `.ts` or `.tsx` work | **typescript-best-practices** |
| Strip comments before review, using a reviewer that didn't write them | **no-comments** |
| Clean AI tells out of prose | **unslop** |
| Write docs, an RFC, a README, a PR description, or a commit message to a standard | **technical-writing** |
| Hear the last reply again in plain words | **bro** |
| Give agents a scripted way to drive the app and prove behavior | **create-verification-skill** |
| Bring a verification doc/skill and its feature map back in line with the app | **maintain-verification-skill** |
| Formally verify a race, invariant, or other property tests rarely reach | **create-formal-verification-skill** |
| Bring a formal verification back in line after the code it covers changed | **maintain-formal-verification-skill** |
| Show a decision-maker a state flow as a chart (note: the model-checking tool itself wasn't ported — see the skill's gap note) | **statechart** |
| Vet a performance number before reporting or acting on it | **benchmark-checklist** |
| Run a large or cross-cutting change, or one to review after stepping away | **figure-it-out** |
| Keep a decision log during a run, and review it afterward | **show-me-your-work** |
| Pick a reasoning-effort budget | **setup-pstack** |
| Turn their own working habits into a personal mode skill | **automate-me** |
| Turn what a finished task taught into skill edits | **reflect** |
| Stop agents from repeating the same mistakes in this repo | **correct** |
| Build a page whose buttons wake a bot teammate over a webhook | **make-bot-ui** |
| Find their way around pstack | **poteto-help** (this skill) |

Close calls:

- **how** explains what the code does. **why** explains the reasons. **teach** runs one or both and explains the result plainly.
- **arena** gives every worker the same brief and merges the best parts. **swarm** splits work into slices or a race and returns one report.
- **interrogate** reviews the diff. **blast-radius** looks for breakage outside the diff and proves the one fact that makes the change safe.
- **recall** rebuilds context across recent chats. **ketchup** covers only this chat since the user's last message.

## Fix a run that went wrong

| Symptom | Fix |
|---|---|
| A question got treated as the next step of the last task | Say "new task", or say the turn doesn't need poteto-mode style. |
| A new reasoning-effort choice had no effect | Memory preferences apply immediately, this chat included — there's no "start a new chat" step needed, unlike the original's rule file. |
| Runs cost more than expected | See the cost paragraph under Get set up. Run **setup-pstack** for a smaller budget. |
| A skill didn't load on its own | Skills load when the user names them, or when poteto-mode style runs them as a playbook step — and it doesn't run every skill. |
| Parallel teammates overwrote each other's work | Give each its own scope or branch, and say so explicitly in the brief. |
| An unattended or delegated run moved but finished nothing | Make sure the goal has a check that can pass or fail, not a duration. |
| The reply claims success from a green build | Ask for the real command output, flow, stored value, or profile. That's the prove-it-works principle. |

## Make pstack my own

- **automate-me** drafts a personal mode skill from the user's own history, to use alongside poteto-mode style.
- **reflect** after a session turns its lessons into skill edits the user approves.
- Asking poteto-mode style to "write a skill for X" runs the authoring playbook.
- Fix a misbehaving skill as its own change, not inside the feature work where it went wrong.

## Reply

Lead with the answer. Give at most one example prompt, adapted from `references/recipes.md` when one fits, then name that file. Keep it short unless the user asked for the whole map. Wording help lives in `references/prompting.md`.