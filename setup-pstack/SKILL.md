---
name: setup-pstack
description: "Configure the default reasoning-effort budget that the ported pstack-style skills (poteto-mode, arena, swarm, interrogate) use for their subagents. Use for \"configure pstack\", \"pstack budget\", or changing how much reasoning effort these skills spend by default."
---

# Setup pstack

The original pstack plugin writes an always-applied Cursor rule file that routes each role (feature work, bug fixes, perf work, review panels, swarm workers) to a specific model slug from several vendors (Claude, Gemini, Grok) at a chosen budget. In this account only Claude (Sonnet 5) is connected — Grok and Gemini model slugs exist in the catalog but aren't usable here — so there is no multi-vendor panel left to route between models. What survives, and what this skill actually configures, is the one real knob left: how much reasoning effort subagents spend by default.

## Steps

1. Ask the user to pick a budget:
   - **large** — xhigh reasoning for feature/bug-fix/perf/judgment-and-prose work, high for swarm workers and review-panel seats.
   - **medium** — high reasoning for feature/bug-fix/perf/judgment-and-prose work, medium for swarm workers and review-panel seats.
   - **small** — medium reasoning for feature/bug-fix/perf/judgment-and-prose work, low for swarm workers and review-panel seats.
2. Record the choice with the memory tool's "prefer" action, as a standing preference every bot reads, e.g.: "Betinho's default reasoning-effort budget for pstack-style skills (poteto-mode, arena, swarm, interrogate subagents) is <budget>: <role>→<effort> for code/judgment work, <role>→<effort> for swarm/panel workers."
3. Tell the user the choice applies from now on, this chat included — there's no per-role model list to show or re-run-to-pick-up-in-a-new-chat step, because there's only one connected model family.
4. Offer once each, if the project doesn't already have one: a project-local verification doc or skill (**create-verification-skill**) and a formal verification skill (**create-formal-verification-skill**).

## Gap

The original's multi-vendor panel mechanics — arena runners spread across Claude/Fable/Gemini, interrogate's cross-vendor reviewers, arena's cross-judge pool drawn from a different vendor than the parent — don't carry over. The ported **arena**, **interrogate**, and **show-me-your-work** skills each already note, in their own text, that only Claude is connected here and substitute reasoning-effort or framing diversity for real vendor diversity. This skill no longer detects available model slugs, writes a rule file, or validates slugs against a detected set, because there's nothing to detect beyond the one connected family.