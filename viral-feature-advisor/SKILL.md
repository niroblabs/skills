---
name: viral-feature-advisor
description: Use when someone has an existing product (PRD, spec, design doc, or a described app/website/game) and wants suggestions — improvements to what exists, plus new features that could make it spread, raise engagement, or market itself through normal use (viral loops, word-of-mouth, shareable artifacts, referral, network effects).
---

# Viral Feature Advisor

## Overview

Take a product that already has a PRD or docs and suggest two things:

1. **Improvements** — refinements to what the product already does.
2. **Growth features** — new features whose *use* spreads the product: shareable artifacts, invites, status, referral, network effects. The goal is **auto-marketing** — the product markets itself when people use it normally.

The reference example: in an "Unseen Bangladesh" travel site, users mark the places they've visited on a map, then share that map on social media. Friends see it, ask about it, and visit the site. Normal use produced marketing. Every growth feature you suggest should have that property.

This skill **suggests** features. It does not build them and does not offer to.

## Steps

### 1. Read the product

Find and read the input before suggesting anything:

- If the user names files or a folder, read those.
- Otherwise look for a PRD / spec / design doc in the project (`*.md` in `docs/`, `README`, a `prd`/`spec` file). Read what you find.
- If the user only described the product in chat and there are no docs, work from the description — don't block on missing files.

If you genuinely can't tell what the product does, ask **one** short question. Otherwise proceed.

### 2. Extract the product's core

Before suggesting, write down for yourself (not necessarily shown):

- **What the product does** and who it's for.
- **The core action** — the one thing users do most, or the "aha" moment.
- **What users would be proud to show** others, or what they already do socially around this space.

Growth features that ignore the core action feel bolted-on. The best ones turn the core action itself into something shareable.

### 3. Find the viral loops

For each suggestion, identify a **loop**: user does something → it reaches other people → some of them become new users → they do the same thing. Use these loop types as a checklist — aim across several, not all of one kind:

| Loop type | Mechanic | Example shape |
|-----------|----------|---------------|
| **Shareable artifact** | Use creates an image/page worth posting | The visited-places map; a year-in-review; a stats card |
| **Status / identity** | A badge, rank, or title people display | "Top 1% explorer", streaks, verified levels |
| **Collaboration** | The thing is better or only works with others | Shared trips, group challenges, co-editing |
| **Invitation** | A task naturally pulls in a non-user | "Tag who you went with", split-a-plan, challenge a friend |
| **Referral incentive** | Both sides get something for an invite | Unlock a feature, credit, early access |
| **User-generated content** | Users make content that ranks in search/social | Reviews, guides, public profiles indexed by Google |
| **Network effect** | Each new user makes it better for existing ones | Leaderboards, maps that fill in, local discovery |

### 4. Prioritize

Rank suggestions by **impact × low effort**, and flag each with a rough effort (S/M/L) and the loop type it drives. Lead with the one or two that best turn the existing core action into a shareable moment.

## Deliver

Two clearly separated sections. Lead with growth features — that's what was asked for.

### A. Growth & auto-marketing features (the main event)

For **each** feature (aim for 4–7, best first):

- **Name** — short and concrete, with an emoji.
- **What it is** — 1–2 sentences.
- **The loop** — the exact chain: user does X → others see Y → they come because Z. Name the loop type.
- **Why it fits this product** — tie it to the product's actual core action, not a generic playbook.
- **Effort** — S / M / L, with the main technical piece in a few words.
- **Watch out for** — the one thing that could make it feel spammy, forced, or privacy-invasive (optional, include when relevant).

### B. Product improvements

A shorter ranked list of refinements to what already exists — UX, clarity, retention, onboarding. For each: one line on the problem, one line on the fix.

End with a one-line **"if you do only one thing"** recommendation.

## Save (always)

After delivering in chat, **always** save the suggestions to a file, without being asked:

- Real timestamp via `date "+%Y-%m-%d %H:%M:%S %Z"` (slug: `date "+%Y-%m-%d_%H%M"`). Never hand-write the time.
- Write to `feature-suggestions/<YYYY-MM-DD>_<HHMM>-<product-slug>.md` in the current working directory (create the `feature-suggestions/` folder if missing).
- Include the date/time, the product name, which docs you read (paths), and all suggestions from both sections.
- Tell the user the file path in one short line.

Then stop. No build offer, no implementation.

## Example feature output (one growth feature)

> ### 🗺️ My Bangladesh Map
> **What it is:** Users check off the districts and spots they've visited; the site generates a personal filled-in map of Bangladesh (and a world map for the ambitious).
> **The loop** *(shareable artifact + network effect)*: user marks their visited places → posts the map to Facebook/Instagram → friends see how much of the country someone has covered and ask about it → they visit the site to make their own → their map fills in over time, pulling them back.
> **Why it fits:** The product is about discovering unseen places; a map of *where you've been* is the natural trophy for that behavior — people already brag about travel.
> **Effort:** M — district/geo dataset + an SVG/canvas map renderer + a one-tap "share as image" export sized for social.
> **Watch out for:** Make the shared image look great on its own (title, count, clean design) — an ugly auto-post won't get shared.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Suggesting features without reading the PRD/docs | Read the input first; anchor every suggestion in the real product. |
| Generic growth playbook ("add referrals, add gamification") | Tie each feature to the product's specific core action and audience. |
| Features with no real loop | State the exact chain back to new users. If you can't, it's not a growth feature. |
| Only one loop type (e.g. all badges) | Spread across loop types — artifact, status, collaboration, invite, referral, UGC, network. |
| Ignoring the "feels spammy" risk | Flag anything that could annoy users or leak private data; good loops feel natural, not forced. |
| Burying the growth features under improvements | Lead with growth features — that's the ask. Improvements are the second section. |
| Offering to build the features | Stop at suggestions. This skill delivers ideas only. |
| Hand-writing the timestamp or skipping the save | Always run `date` and always save the file. |
