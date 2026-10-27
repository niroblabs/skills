---
name: viral-idea-generator
description: Use when someone wants viral product ideas for a specific market (a game or a website/app), OR wants to research viral content — understand why a specific viral thing blew up, or find trending/viral content matching a search term or topic. Covers trend research, trend arbitrage (importing what's viral elsewhere), and cultural localization.
---

# Viral Idea Generator

## Overview

This skill has **two modes**:

- **Research mode** — find out what's viral and why. The user wants to understand a specific viral thing, or discover viral content matching a term/topic. **Output is research only** — no ideas unless the user asks for them next.
- **Idea mode** — turn trends into **2 fresh viral ideas** tuned for one target market, via trend arbitrage (carry a winning mechanic over from elsewhere) + localization (make it feel native).

In idea mode the output is **ideas only** — do not build the game or app, and do not offer to.

## Choose the mode first

Read the request and pick:

- Wants to **know / find / understand / research** viral content, or search a term → **Research mode**.
- Wants an **idea / something to build / a concept** → **Idea mode**.
- Ambiguous → ask one short question: *"Do you want me to research what's viral, or generate ideas to build?"*

---

## Research Mode

### What the user is asking

Two shapes — handle whichever fits:

- **Specific content** (e.g. "research the slapman video"): explain what it is, a rough timeline, where it spread, and *why it went viral* (the mechanic, the emotion, the shareable moment), plus its current status.
- **Term / topic search** (e.g. "find viral content about street food in Dhaka"): return a ranked list of matching viral/trending items.

### Steps

1. **Scope.** If the user names or implies a place, scope to it; otherwise research broadly and say so. Market is **not** required in research mode — don't force the question.
2. **Search the web** across a spread of sources (news, Reddit, app/Play-Store charts, Product Hunt, YouTube, TikTok/Instagram chatter, Hacker News). Favor the last few weeks. Cast wide; no preset source list.
3. **Be honest about confidence.** If you can't confirm a specific item, say so and give your best read rather than inventing specifics.

### Deliver

- **Specific content:** `What it is` · `Timeline` · `Where it spread` · `Why it went viral` · `Current status`.
- **Term search:** a short ranked list; for each item give `What it is` (one line) · `Why it caught on` · `Where it's spreading`.
- Always include sources as markdown links.

### Save (always)

Save findings to a file, every time, without being asked:

- Real timestamp via `date "+%Y-%m-%d %H:%M:%S %Z"` (slug: `date "+%Y-%m-%d_%H%M"`). Never hand-write the time.
- Write to `viral-research/<YYYY-MM-DD>_<HHMM>-<topic-slug>.md` in the current working directory (create the folder if missing).
- Include the date/time, the query/topic, the scope, the findings, and the sources.
- Tell the user the file path in one short line.

Then stop. You may add **one** short line offering to turn the research into ideas — do not generate ideas unless the user says yes.

---

## Idea Mode

### 1. Get the market

The target market is required. If the user didn't name one, ask a single question: *"Which market are you launching this in — a country, city, or region?"* Wait for the answer before researching.

### 2. Gather trends

Figure out which mode you're in:

- **User brought trends** → study the things they pasted. Work out *why each one caught on* (the mechanic, the emotion, the shareable moment). You may still search the web to understand an unfamiliar reference.
- **User brought nothing** → research trends yourself with web search.

Always research **both directions** when you search:

- **In-market:** what is viral *in the target market right now* — ride the existing wave.
- **Elsewhere:** what is blowing up in *other* markets that hasn't reached the target yet — import and localize it.

Search across a spread of sources, not one: news, Reddit, app-store / Play-Store charts, Product Hunt, YouTube, TikTok/Instagram chatter, Hacker News. Favor recent (last few weeks). Cast wide.

### 3. Generate exactly 2 ideas

Each idea is **one game OR one website/app** (no other types for now). Make them culturally specific to the market, not generic. Prefer two genuinely different bets over two variations of the same thing.

### 4. Deliver — one idea per block, in this shape

For **each** of the 2 ideas, produce every part below, in this order:

- **Name** — catchy, culturally tuned, with an emoji.
- **Type** — Game or Website/App.
- **Inspired by** — the trend(s) it draws on, and whether it's in-market or imported.
- **Pitch** — what it is, in 2–3 sentences.
- **Why this market** — the specific cultural reason it lands here (humor, habit, language, shared reference).
- **Viral hook** — the one concrete reason people would share it.
- **Build sketch** — a rough sense of what it takes to make (one or two lines).

### 5. Save the ideas to a file (always)

After delivering the ideas in chat, **always** save them to a file. Do this every time ideas are generated, without being asked.

- Real timestamp via `date "+%Y-%m-%d %H:%M:%S %Z"` (slug: `date "+%Y-%m-%d_%H%M"`). Never hand-write the time.
- Write to `viral-ideas/<YYYY-MM-DD>_<HHMM>-<market-slug>.md` in the current working directory (create the `viral-ideas/` folder if missing).
- Put the generation date and time, the market, and all parts of both ideas in the file. Include the sources.
- Tell the user the file path in one short line.

Then stop. No build offer, no implementation.

## Example Idea Output (one of the two ideas)

> ### Idea 1 — Rickshaw Rush 🛺
> **Type:** Game
> **Inspired by:** "Baper Rasta" driving-chaos humor (in-market) + a one-tap traffic game trending in Brazil (imported).
> **Pitch:** A one-tap mobile game where you weave a rickshaw through Dhaka traffic — dodging buses and potholes, picking up passengers for points. Leaderboards by city.
> **Why this market:** Dhaka traffic is a shared national joke; the frustration is instantly relatable and funny to every Bangladeshi.
> **Viral hook:** Shareable "I survived 47 seconds in Dhaka traffic" score cards built for Facebook and WhatsApp.
> **Build sketch:** HTML5 canvas game, single-tap control, ship as a web link first, native app later.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Generating ideas when the user only wanted research | Pick the mode first. Research mode stops at findings. |
| Forcing the market question in research mode | Market is optional for research; only ideas require it. |
| Researching for ideas without knowing the market | In idea mode, ask for the market first; localization is the whole point. |
| Only looking at in-market trends | Always also scan other markets to import — that's the arbitrage edge. |
| Generic ideas that fit anywhere | Anchor each idea in a specific local reference, habit, or joke. |
| Offering to build the thing | Stop at the idea. This skill delivers concepts only. |
| Producing 1 or 3+ ideas | Always exactly 2 in idea mode. |
| Idea types beyond game/app | Keep to games and websites/apps for now. |
| Hand-writing the timestamp or skipping the save | Always run `date` and always save (ideas and research both). |
