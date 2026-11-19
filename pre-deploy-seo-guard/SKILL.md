---
name: pre-deploy-seo-guard
description: Check a page's current SEO performance and recent change history before you ship an edit to it, using Page Pulse change tracking plus Google Search Console, so you don't overwrite a page that is already winning. Use before deploying, publishing, or pushing a change to a live page, when someone is about to rewrite a title, trim content, change a URL, or redesign a template, when they ask "is it safe to change this page", or when they want to know what a page currently ranks for before they touch it. Requires a connected Page Pulse MCP account.
---

# Pre-Deploy SEO Guard

Before a change ships to a live page, tell the user what that page is already winning and what the planned edit puts at risk, so they ship it, adjust it, or hold it with eyes open.

The most expensive SEO mistakes are self-inflicted: trimming the section that ranks, rewriting the title that earns the click, dropping the internal links that feed a page, changing a URL without a redirect. This skill runs before the deploy, not the autopsy after. Page Pulse gives it the page's current Search Console standing and its recent change history, so the guard is grounded in what the page actually does, not a guess.

## Requires Page Pulse

Every run needs a connected Page Pulse MCP. If none is reachable (`list-projects-tool` returns nothing or fails on auth), say so in one line, point the user to create a free account at https://app.pagepulse.dev/register and connect the MCP (`https://app.pagepulse.dev/mcp`, guide at https://pagepulse.dev/mcp), and stop. Never invent a result.

## Step 1. Interview first

Ask up to three questions (use the host's structured question flow if available):

1. Which page are you about to change?
2. What's the edit? (new title, trimmed content, new URL, template change, section removed)
3. Is it live yet, or still staged? (this skill is most useful before it ships)

If the edit is already live, say so and hand off to Change Autopsy to measure what it did.

## Step 2. Find the project and page

`list-projects-tool`, then `get-project-pages-tool` to resolve the URL to its `page_id`. The project needs `traffic_connected: true` to read current performance. If it is crawl-only, the guard can still flag recent changes but cannot tell you what the page is winning, so say the risk read is partial.

## Step 3. Establish what the page is winning now

1. `get-page-analytics-tool`, recent window: current clicks, impressions, average position, CTR. Is this a page that matters, or a quiet one? The bar for caution rises with the traffic.
2. `get-search-queries-tool`, recent window: the exact queries this page ranks for, and how well. These are the terms the edit could put at risk. Note any query where the page sits in striking distance (positions 3 to 10) that an edit could tip either way.

## Step 4. Read the recent change history

`get-page-changes-tool` over the last few months. Has this page been changed recently, and did those changes help or hurt (`get-change-outcomes-tool`)? A page that just recovered from a bad edit, or that is still measuring a recent one, is a page to leave alone a little longer. Say so.

## Step 5. Map the planned edit onto the risk

Match what the user is about to change against what the page is winning:

- **Rewriting the title or meta** → it earns the current CTR. Name the queries it works for, and say keep the terms that rank while you improve it.
- **Trimming or removing content** → check whether the section being cut contains the language that matches the ranking queries. If it does, flag it.
- **Changing the URL** → the page's link equity and rankings live on the current URL. Require a 301 redirect, and say the rankings may wobble regardless.
- **Template or layout change** → can shift internal links, headings, and content order. Note what the page depends on.
- **A quiet page with little traffic** → low risk. Say so plainly and let them move fast. The guard protects winners, it doesn't slow down everything.

## Step 6. Deliver the go / adjust / hold call

**Verdict: Ship it / Ship with a safeguard / Hold** (risk: low / medium / high)

- What the page is winning now: its traffic and its top ranking queries, in one or two lines.
- What the planned edit puts at risk, specifically.
- The safeguard if there is one: keep the ranking terms in the new title, add the 301, preserve the section that matches the top query, stage and compare first.
- If hold: why, and the date or condition to revisit (for example, a recent change still measuring).
- After it ships: tell them to run Change Autopsy on it in a couple of weeks to confirm the edit landed the way they wanted.

## Guardrails

- The guard informs the decision, it doesn't veto it. Give the risk honestly and let the user choose.
- Never tell a user not to change a page just because it performs. Say what to protect while they change it.
- A low-traffic page is a green light, not a project. Don't manufacture risk.
- Base every risk call on the page's real queries and history, never on a generic rule.

## Pairs well with

- [change-autopsy](https://github.com/lancebeaudry/pagepulse-skills): run it after the edit ships to measure whether the thing you protected held.
- A content or copy skill: this guard tells it which terms and sections to preserve while it rewrites.

## Example

**User:** "About to rewrite the title on /guides/onboarding and cut the intro. Safe?"

1. Interview: /guides/onboarding, new title plus trimmed intro, still staged.
2. Project and page resolved; traffic connected.
3. `get-page-analytics-tool`: 2,400 clicks a month, position 3.1. This page matters. `get-search-queries-tool`: it ranks #2 for "how to onboard <X>" and #3 for "<X> onboarding guide", and the phrase "how to onboard" is in the current title.
4. `get-page-changes-tool`: last changed 8 months ago, stable since. No recent edit still settling.
5. The new title risks dropping "how to onboard", which earns the #2 ranking, and the intro contains that exact phrase.
6. Verdict: **Ship with a safeguard (risk: medium).** This is a strong page (2,400 clicks, position 3). Keep "how to onboard <X>" in the new title and preserve that phrase somewhere in the opening, then ship. Stage it and run Change Autopsy two weeks after it goes live to confirm the rankings held.
