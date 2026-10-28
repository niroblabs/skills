---
name: ad-hunter
description: Use when given a URL — a landing page, redirect, checkout, or
  "pay" link like pay.example.com — and you need to find the paid ads
  pointing at it and where they run (Google, Meta/Facebook/Instagram,
  TikTok, LinkedIn, YouTube, Bing). Covers tracing redirects, reading
  click-IDs and tracking pixels to see which ad platforms a site uses,
  searching public ad-transparency libraries by advertiser or domain, and
  pulling the actual ad creatives. For competitor ad research, ad spying,
  and checking what campaigns a brand is running.
---

# Ad Hunter: Ad Intelligence Investigator

## Overview

You are an investigator working a **chain of evidence**, not a search box.
A URL by itself tells you almost nothing; the ads that point to it live in
public ad-transparency libraries that are keyed to an **advertiser or a
domain**, not to a random subdomain like `pay.example.com`. Your job is
to turn the URL into the real advertiser, read the signals the site leaks
about which platforms it buys ads on, then look each ad up in its library
and bring back the creatives.

**Core principle:** You cannot find ads for a URL directly. You resolve the
URL to a *brand/domain*, confirm *which platforms* it advertises on from
hard signals, then query *that platform's library*. Skipping to "search the
ad library" without resolving and confirming first produces empty results
and wrong conclusions.

**Research before asserting.** Every claim about where ads run must come
from a redirect, a pixel, a click-ID, or a library result you actually
pulled — never from memory. Use WebFetch/WebSearch and the browser.

## The gated pipeline

```
INTAKE → 1 Resolve the advertiser → 2 Read platform signals
       → 3 Search each ad library → 4 Pull creatives → 5 Report
```

### Stage 0 — Intake
Take the URL from the user. That's the only required input. Note anything
they add (a brand name, a country, a platform they already suspect).

### Stage 1 — Resolve the real advertiser
The libraries index the **brand/root domain and legal advertiser**, not the
subdomain. So:
- Follow the full redirect chain (`WebFetch`, or `curl -sIL` via Bash). A
  "pay"/"go"/"track" link usually 301/302s to the real destination — and
  the redirect URL often carries the UTM/click-ID that names the platform.
- Reduce to the **root domain** (`pay.example.com` → `example.com`)
  and the **brand name** (Acme).
- Find the **legal advertiser name**: footer, Terms/Privacy pages, "©
  Company Ltd", WHOIS, app-store listing. Google and Meta list ads under
  this verified name, which is often not the brand.

### Stage 2 — Read platform signals (which networks they buy)
Before searching, confirm where ads actually run. Two hard sources:

**a) Click-ID & UTM parameters** on the landing/redirect URL:

| Parameter | Platform |
|-----------|----------|
| `gclid`, `gbraid`, `wbraid` | Google Ads (Search/Display/YouTube) |
| `fbclid` | Meta — Facebook / Instagram |
| `ttclid` | TikTok |
| `msclkid` | Microsoft / Bing Ads |
| `li_fat_id` | LinkedIn |
| `twclid` | X / Twitter |
| `epik` | Pinterest |
| `ScCid` | Snapchat |
| `irclickid` | Impact (affiliate network) |
| `utm_source=` | self-declared source (facebook, ig, google, tiktok, bing…) |

**b) Tracking pixels in the page source** (fetch the HTML, grep for these).
A pixel means the platform is wired up for ads — strong signal to check it:

| Signal in source | Platform pixel |
|------------------|----------------|
| `connect.facebook.net`, `fbq(` | Meta Pixel |
| `googleadservices.com/pagead/conversion`, `AW-`, `gtag(` | Google Ads |
| `analytics.tiktok.com`, `ttq.` | TikTok Pixel |
| `bat.bing.com`, `uetq` | Microsoft/Bing UET |
| `snap.licdn.com`, `_linkedin_partner_id` | LinkedIn Insight |
| `static.ads-twitter.com`, `twq(` | X / Twitter |
| `s.pinimg.com`, `pintrk(` | Pinterest Tag |
| `sc-static.net`, `snaptr(` | Snapchat |

List the platforms with a signal. Those are the libraries worth searching
first; the rest are a lower-priority sweep.

### Stage 3 — Search each ad library
Search by the resolved **domain first**, then the **advertiser name**, then
the **brand** as keyword. Domain searches are the most reliable.

| Library | URL | Covers | Search by | Scope |
|---------|-----|--------|-----------|-------|
| **Meta Ad Library** | facebook.com/ads/library | Facebook, Instagram, Messenger, Audience Network | Advertiser/Page name, keyword | **Global, all active ads** (has a public API) |
| **Google Ads Transparency Center** | adstransparency.google.com | Search, Display, YouTube, Shopping, Maps | Advertiser name **or domain** | Global; verified advertisers |
| **TikTok Commercial Content Library** | library.tiktok.com | TikTok | Advertiser name, keyword | **EU/EEA + UK, CH, Türkiye only** |
| **LinkedIn Ad Library** | linkedin.com/ad-library | LinkedIn | Advertiser name | Global; ~last 12 months |
| **X / Twitter** | — | X | — | No reliable public library (discontinued) |
| **Microsoft/Bing** | — | Bing/Audience | — | No public ad library |

These pages are JavaScript-heavy. Prefer the **browser tools** (navigate +
snapshot) over `WebFetch`, which often returns an empty shell. If browser
tools aren't available, say which libraries you couldn't render and give the
user the exact search URL to open themselves.

Meta's URL pattern: `facebook.com/ads/library/?active_status=all&ad_type=all&country=ALL&q=<brand>`.
Google's: `adstransparency.google.com/?region=anywhere&domain=<domain>`.

### Stage 4 — Pull the creatives
For each ad found: capture a **screenshot** (browser tool), and record the
headline/primary text, format (image/video/carousel), the destination URL it
links to (confirm it matches the input), platform/placement, status
(active/inactive), and first-seen/last-seen dates and run length if shown.
Meta also shows how many variations are running.

### Stage 5 — Report
Lead with the answer: **is this URL being advertised, and where.** Then:
- **Verdict line:** "Ads found on: Meta (12 active), Google (3 active). No
  TikTok/LinkedIn ads found." or "No public ads found" with what you checked.
- **Evidence:** the resolved domain/advertiser, the redirect chain, and the
  click-ID/pixel signals that proved each platform.
- **Per platform:** a short table of the live ads with creative screenshots,
  copy, format, dates, and a direct link to each ad in its library.
- **Caveats:** scope limits (e.g. TikTok library is EU-only, so a US-only
  TikTok campaign won't appear), and that libraries show only *disclosed*
  ads — absence isn't proof they aren't advertising.

## When the libraries come up empty
- Re-check the advertiser name — ads are often under a parent company or an
  agency, not the brand (use the WHOIS/footer name).
- Try the brand as a **keyword** and skim for look-alike advertiser pages.
- The campaign may be outside a library's scope (TikTok/X/Bing have big
  gaps, or the ads run only in a country the library doesn't cover).
- **Fallback — third-party ad-spy tools** (broader coverage, mostly paid):
  Meta Ad Library (free) → BigSpy, AdSpy, Minea, Foreplay (social);
  SEMrush, SpyFu (search); Adbeat, SimilarWeb (display). Name these to the
  user; don't claim results from a tool you didn't actually query.

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Searching the library for `pay.example.com` | Resolve to the root domain + legal advertiser first |
| Concluding "no ads" from one library | The ad is likely on a network you didn't check; sweep all, note scope gaps |
| Trusting `WebFetch` on the library pages | They're JS apps — use the browser tools and screenshot the creatives |
| Ignoring the redirect's own URL | The UTM/click-ID in it often names the platform outright |
| Reporting platforms from pixels alone | A pixel means *set up to advertise*, not *currently running* — confirm in the library |
| Asserting ad spend or targeting | Libraries rarely show spend/targeting outside political ads; don't invent it |
