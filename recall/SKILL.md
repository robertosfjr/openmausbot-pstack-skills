---
name: recall
description: "Reconstruct your recent working context from your own chat history, live state, and the shared record (user reports, prior fixes, incidents), then hand back a tight current-state brief. Use for 'recall my work on X', 'catch me up', 'what have I been working on', 'where did I leave off', before starting or resuming work."
---

# Recall

**Before you start or resume work, you rebuild the user's recent working context and hand back a tight capsule of where things stand now and what to do next.**

Keep it tight and on-topic. Read only what the in-scope threads need, then stop.

Your context lives in two records. Your own chat history holds what you did and decided. The shared record holds everything that happened around the same code under other names: the symptoms users keep reporting, the fixes that shipped and got reverted, the errors still firing in prod. That second record lives in source control, the issue tracker, chat and issue channels, long-form docs, and error tracking — in this workspace, reached through the connected apps and tools you already have (GitHub via `gh`, any connected chat or ticket app, etc.), not a separate mining step. A feature with a long bug tail keeps most of its story there, so don't reconstruct it from your own history alone.

The original pstack plugin mines raw transcript files at `~/.cursor/projects/<slug>/agent-transcripts/<uuid>/<uuid>.jsonl`. OpenMausBot has no equivalent raw-file corpus to grep. Use the `session_search` tool instead: it searches your own earlier conversations, rooms, and memory files by keywords or a time range.

1. Classify, then route. One specific prior chat to resume is a different task (picking up a named thread), not this. Turning habits into a durable skill is **automate-me**. A human-readable summary of your work is a different task. Recall loads working context across recent chats before you act. If the user already gave you a full state capsule (paths, branch, the change), use it and skip the mining.
2. Lock the scope before searching. Pin the window ("recent" is a real range, default the last 7 days), the topic if named, and the workspace or project (default the active one. Never pull in another project's history without being asked). State the scope back. Never quietly turn "all" into "recent N".
3. Search your own history. Call `session_search` with the topic's keywords and the time range. For a broad or unclear topic, run it a few times with different keyword angles rather than fanning out to parallel subagents the way the original does over a raw file corpus — `session_search` already does the search server-side. Order candidates by recency, read only the matching parts, and skip the current chat plus obvious noise (subagent, eval, and test threads). Pick each subagent's reasoning level for the job, per the Subagents section of the **poteto-mode** skill, if you do delegate a slice to a teammate.
4. Sweep the shared record whenever the topic names a feature, file, subsystem, area, or bug. This is the default, not a judgment call, and "my work on X" does not exempt it. For each relevant connected source (GitHub, a ticket tracker, a chat app), ask "what's the current state, what's been tried and didn't hold, and what are users still reporting". Null results are findings. Skip a source you can't reach and say so. Fold what comes back into the brief. Skip this step only for pure activity recall with no named target ("what did I do this week"), where your own history and live state are the entire answer.
5. Verify against live state. Take the PRs, branches, and tickets that the search and the sweep surfaced and check them with `git` and `gh`. When the answer hinges on what an agent actually did (the tools it ran, files it read, errors it hit), read the full session via `session_search`, not just a trimmed summary.
6. Write the brief to the contract below. Group by thread. Stay on the named topic.

## Output contract

Lead with the capsule, then the thread status, then the problems, then the next move. Deeper detail goes below or gets cut.

- **Capsule.** At most 5 bullets. What this work is and where it stands overall.
- **Threads.** One line each, prefixed with exactly one status tag: `[merged #N]`, `[open PR #N]`, `[in flight <branch>]`, `[verified, uncommitted]`, `[reverted #N]`, or `[planned, not started]`. A thread with no tag is not done yet, so tag it.
- **Problems.** At most 5, the recurring ones. Include the symptoms users keep reporting and any fix that shipped and was reverted, so the next attempt starts where the last one failed.
- **Next move.** The single most useful next action, concrete.

An adjacent feature or ticket stays out unless it blocks this one. When the capsule and thread lines outgrow a screen, cut detail before you cut threads. Write the brief through the **unslop** skill, cite findings by their source (PR #, ticket ID, chat permalink, error-tracker issue, or session reference), and sanitize private context before any public output.

**Reply:** the brief, to the contract above.