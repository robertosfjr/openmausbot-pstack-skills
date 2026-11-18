# Prompts worth copying

From pstack's recipes guide. Swap in the real paths, skills, and done checks. Informal wording works. Where the original used a Cursor slash command for a skill that was ported here under the same name, the phrasing below keeps that skill name.

## Understand

- "read <thread>, in poteto-mode style. restate the underlying issue in your own words, in plain english."
- **in-your-own-words** on a long or messy ask.
- **sharpen** on the prompt you're about to send.
- "in poteto-mode style, investigate why <symptom>. give me what we know, what data you used, and your best hypotheses. don't change any code yet."
- "use **how** to understand <subsystem>. then use **why** to find out why it broke recently."
- "**recall** my work on <topic> from last week, then read <issue>."
- "**teach** me why you implemented it this way and not <other way>. what did you trade off?"
- "in poteto-mode style, take over this branch. read the decision log, find what's done, and continue. don't redo finished work."

## Build

- Bug: "in poteto-mode style, <symptom>. repro first, then fix and verify."
- Bug in an app: "in poteto-mode style, repro this with the verification skill for <app>. if it repros on main, fix it and show me a video as proof."
- Bug with a cheap test: "in poteto-mode style, repro <bug> first. if there's a cheap test path, use **tdd**. then fix and rerun."
- Feature: "in poteto-mode style, add <behavior>. <current output> stays byte-identical. verify both."
- Refactor: "in poteto-mode style, move <code> into one module, zero behavior change. record the current output first and prove it's unchanged after."
- Perf: "in poteto-mode style, <operation> takes <time> on <fixture>. trace it, fix the measured cause, show me before and after."

## Design and plan

- "in poteto-mode style, prototype a few options for <feature>. take screenshots or videos for me to compare."
- "in poteto-mode style, we need <feature>. use **architect** first, and answer open questions with prototypes. let me review before proceeding."
- "in poteto-mode style, write a tutorial for how i would use <new package> first. then **teach** me why it beats the current one."
- "ask **arena** for a second opinion on this thread and our approach."
- "in poteto-mode style, turn this design into a plan. small verifiable PRs, each with its own verification steps."

## Review and ship

- "**interrogate** the whole branch, but skeptically. don't change anything yet. no nitpicks unless it's a real bug or regression." Read the Dropped list too.
- "**create-formal-verification-skill**: prove <property> holds in <module>, even when <steps> interleave or retry." After merges or refactors in that code: **maintain-formal-verification-skill**.
- "**swarm**: check every package under <dir> against its check script. one worker per package. one report."
- "in poteto-mode style, open the pr. small ordered commits, evidence in the description."
- "in poteto-mode style, babysit this pr. get it green." For status only: "in poteto-mode style, check on pr <number>. anything outstanding?"

## Away and back

- "in poteto-mode style, im going to bed. <goal> in a fresh branch off <base>. done means <checks>. keep a decision log. don't ask me before committing. keep going until done. if you're truly stuck after a few hours, stop and write up why."
- "**show-me-your-work**: catch me up on what you did last night." Read its Attention section first.
- **ketchup** catches you up on this chat since your last message and lists every call that waits on you.
- "**reflect**: capture what we learned so the next run doesn't repeat it." Approve only edits that change a future decision.
- **bro** restates the last reply in plain words.
