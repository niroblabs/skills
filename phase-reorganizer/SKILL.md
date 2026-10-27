---
name: phase-reorganizer
description: Use when a project has a PRD with a phased plan or roadmap and someone wants the phases re-organized against reality — detect what's actually done (including items finished ahead of schedule from later phases), pull all completed work into one bucket, and re-divide the remaining work into new phases ranked by priority, user urgency, and feature value.
---

# Phase Reorganizer

## Overview

Plans drift during implementation. Items from phase 3 get done while phase 1 is still open; some phase-1 items get skipped. This skill re-syncs the plan with reality:

1. **Detect what's actually done** — across *all* phases, not just the current one.
2. **Collect completed work** into one "Done" bucket regardless of which phase it was originally in.
3. **Re-divide the remaining work** into new phases, ordered by priority, user urgency, and feature value (and dependencies).

The output is a **re-organized phase plan**. This skill plans; it does not implement the work.

## Steps

### 1. Read the PRD and the current phases

- Read the PRD/spec and the phase plan (ask the user for the file, or look for `*.md` in `docs/`, a `README`, a `roadmap`/`plan`/`prd` file).
- Build a flat list of every work item across every phase, tagged with its **original phase**.

### 2. Detect what's actually done

For each item, decide **Done / Partial / Not done** from *evidence*, in this order:

1. **Explicit status** — checkmarks, "✅/[x]", "DONE", a status column, a changelog.
2. **Code / artifact evidence** — the feature exists in the codebase, a file/route/endpoint/test is present. Search the repo to confirm; don't assume.
3. **Git history** — recent commits that implement an item.

Mark confidence. **Never guess "done" silently.** If evidence is ambiguous for an item, list it as **Needs confirmation** and ask the user about those items in one batch — don't interrogate item by item.

### 3. Build the "Done" bucket

Collect every Done item into one completed list, **regardless of its original phase**. For each, note the original phase it came from (e.g. "was Phase 3") so the drift is visible. Partial items get split: the finished part goes to Done, the remainder becomes a new item for re-planning.

### 4. Re-prioritize and re-divide the remaining work

Score each remaining (Not done / remainder) item on these factors, then group into phases most-important-first:

| Factor | Question | Weight |
|--------|----------|--------|
| **User urgency** | Do users need this now / are they blocked without it? | high |
| **Feature value** | How much does it move the product's core goal? | high |
| **Stated priority** | What did the PRD/user already rank it as? | medium |
| **Dependency** | Does other work depend on this? Must it come first? | gate |

Rules:
- **Dependencies gate order** — a blocker ships before the things that need it, even if those are flashier.
- **Phase 2 = must-have next**; each later phase is genuinely less necessary than the one before it. Don't flatten everything into "important."
- Keep phases roughly balanced in size unless a dependency forces otherwise.
- If the user stated an urgency or a deadline, honor it over the generic ranking.

## Deliver

Present in this order:

### A. Reality check (short)
One or two lines: how much of the plan is actually done, and the notable drift (e.g. "3 of Phase 3's items are already done; 2 Phase-1 items are still open").

### B. ✅ Done
The completed items, each with its original phase in parentheses.

### C. 🔄 Needs confirmation (only if any)
Items whose done-ness you couldn't verify, as a short list of yes/no questions. If this section is non-empty, say the re-divided plan below is provisional until confirmed.

### D. Re-organized phases
`Phase 2`, `Phase 3`, … each with its items. For every item give a one-line **why it's in this phase** (the dominant factor: urgency / value / dependency). Note any dependency explicitly ("needs X from Phase 2").

End with a one-line **"do next"** pointing at the single highest-value unblocked item.

## Save (always)

After delivering in chat, **always** save the re-organized plan to a file, without being asked:

- Real timestamp via `date "+%Y-%m-%d %H:%M:%S %Z"` (slug: `date "+%Y-%m-%d_%H%M"`). Never hand-write the time.
- Write to `phase-plans/<YYYY-MM-DD>_<HHMM>-<project-slug>.md` in the current working directory (create the `phase-plans/` folder if missing).
- Include the date/time, which docs/sources you read (paths), the Done bucket, any needs-confirmation items, and the full re-organized phases with the "why" per item.
- Tell the user the file path in one short line.

Then stop. No implementation.

## Example output (excerpt)

> **Reality check:** ~55% of the plan is done. 2 of Phase 3's items shipped early; 1 Phase-1 item (email verification) is still open.
>
> **✅ Done**
> - User signup & login *(Phase 1)*
> - Map rendering *(Phase 1)*
> - Share-as-image export *(was Phase 3 — done early)*
> - Public profile pages *(was Phase 3 — done early)*
>
> **🔄 Needs confirmation**
> - Is push-notification wiring actually working end-to-end, or just stubbed?
>
> **Re-organized phases**
> **Phase 2 — unblock core use**
> - Email verification — *urgency: users can sign up but can't recover accounts; also blocks password reset.*
> - Password reset — *value + dependency: needs email verification first.*
>
> **Phase 3 — growth**
> - Visited-places leaderboard — *value: drives the sharing loop, but not blocking anyone today.*
>
> **Do next:** Email verification — highest-value unblocked item and a dependency for Phase 2.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Marking items done without evidence | Check docs, code, and git; list unclear ones as Needs confirmation. |
| Keeping items in their original phase | Re-sort everything; the whole point is to re-divide against reality. |
| Leaving done items scattered across phases | Pull all completed work into one Done bucket, noting original phase. |
| Flattening everything into "high priority" | Later phases must be genuinely less necessary than earlier ones. |
| Ignoring dependencies | A blocker ships before what depends on it, even if less flashy. |
| Interrogating the user item by item | Batch all confirmation questions into one short list. |
| Starting to implement the work | Stop at the re-organized plan. This skill only replans. |
| Hand-writing the timestamp or skipping the save | Always run `date` and always save the file. |
