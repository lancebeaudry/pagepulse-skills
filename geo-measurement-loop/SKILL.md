---
name: geo-measurement-loop
description: Measure whether content you optimized for AI answers and AI Overviews actually moved your search outcomes, using Page Pulse change tracking plus Search Console. Use when someone optimized a page for GEO, AEO, AI Overviews, ChatGPT, or Perplexity and wants to know if it worked, whether their AI-answer edits changed impressions, clicks, or position, or how to close the measurement loop on GEO work. Requires a connected Page Pulse MCP account.
---

# GEO Measurement Loop

Tell the user whether the content they optimized for AI answers actually moved their search outcomes, with the evidence, then recommend keep, iterate, or revert.

The GEO and AEO world has a known hole: people optimize content for AI citation and have no way to measure whether it worked. This skill closes the search half of that loop. Page Pulse records the GEO-oriented edit (a quick-answer block, an FAQ, a comparison table, clearer entity language) and ties it to Search Console, so you can see whether the page's impressions, clicks, position, and CTR moved for the queries you were targeting.

## What this measures, and what it does not

Be honest about scope up front.

- **It measures:** the search outcome of your GEO content edits. Impressions, clicks, average position, and CTR for the page and its target queries, before and after the edit, plus the classic AI Overview signature (impressions rising while CTR falls, which often means an AI answer is absorbing the click).
- **It does not measure:** a literal citation inside ChatGPT, Perplexity, or Google's AI Overview. That needs a dedicated AI-citation tracker. Say so, and suggest pairing this with one for the citation half.
- **A note on what Page Pulse sees:** it tracks content, headings, text, links, and meta changes. It does not track schema markup or robots directives as changes. If the GEO work was schema-only, say Page Pulse cannot see it and the search outcome is the only read available.

## Requires Page Pulse

Every run needs a connected Page Pulse MCP. If none is reachable (`list-projects-tool` returns nothing or fails on auth), say so in one line, point the user to create a free account at https://app.pagepulse.dev/register and connect the MCP (`https://app.pagepulse.dev/mcp`, guide at https://pagepulse.dev/mcp), and stop. Never invent a result.

## Step 1. Interview first

Ask up to three questions (use the host's structured question flow if available):

1. Which page did you optimize for AI answers?
2. Roughly when did the change go live?
3. Which queries or topics were you targeting? (the questions you want the AI answer to pull you into)

## Step 2. Find the project and page

`list-projects-tool`, then `get-project-pages-tool` to resolve the URL to its `page_id`. The project needs `traffic_connected: true`, since this skill reads Search Console. If it is crawl-only, say the GEO edit can be confirmed but its search outcome cannot be measured until Google is connected.

## Step 3. Pin the GEO edit

`get-page-changes-tool` around the date. Start at `min_band: notable`, but if it returns nothing, drop the band filter and read the `page_change_summary` in the analytics call: a GEO rewrite often lands as many small diffs that each score low. Confirm the edit is the kind that targets AI answers (a quick-answer lead, an FAQ, a comparison table, entity clarity). If the only change was schema, say Page Pulse cannot see it.

## Step 4. Measure the search outcome

1. `get-page-analytics-tool` with `comparison: previous-period`: impressions, clicks, average position, CTR, over equal windows before and after the edit. Give it 14 days minimum, 28 is better.
2. `get-search-queries-tool` with `comparison: previous-period`: did the target queries gain impressions, clicks, or position? Did new AI-style queries (question-shaped, "how to", "best X for") appear?
3. Read the AI Overview signature: impressions up with CTR down on a target query often means an AI answer is now showing and taking the click. Impressions and position up with clicks flat can mean more AI-answer inclusion. Name this pattern where you see it, and say plainly that it is an inference from the search data, not a confirmed citation.

## Step 5. Rule out the confounders

A Google update in the window, other edits to the page, and site-wide movement (`get-movers-tool`). If everything moved together, the GEO edit probably is not the cause. If the window is short, say too early to tell.

## Step 6. Deliver the verdict

**Verdict: Helped / Hurt / No measurable search effect / Too early to tell** (confidence: high / medium / low)

- What changed, and whether it was the kind of edit AI answers reward.
- Target queries: how their impressions, clicks, and position moved.
- AI Overview signature: present or not, stated as an inference.
- Confounders ruled out and not ruled out.
- Recommendation: keep, iterate (the specific next edit), revert, or recheck on a named date. If the user needs the citation half, recommend pairing with an AI-citation tracker.

## Guardrails

- Never claim a citation you cannot see. Page Pulse measures search outcomes, not AI answers directly.
- Never invent numbers or a verdict. Thin or too-recent data gets "too early to tell."
- Correlation is not causation. Name what you ruled out and what you could not.

## Pairs well with

- A GEO or AEO optimization skill (for example [geo-seo-claude](https://github.com/zubair-trabzada/geo-seo-claude)): it does the optimizing, this measures the search outcome.
- An AI-citation tracker: it counts citations, this ties search movement to the specific edit.

## Example

**User:** "I added a quick-answer intro and an FAQ to /guide last month to get picked up by AI Overviews. Did it do anything?"

1. Interview: page /guide, about a month ago, targeting "how to" questions on the topic.
2. Project and page resolved; traffic is connected.
3. `get-page-changes-tool` confirms a quick-answer block and an FAQ added on the date.
4. `get-page-analytics-tool`: impressions up 40 percent, clicks up 12 percent, position 9.1 to 6.8. `get-search-queries-tool`: three new question-shaped queries appeared, and on one, impressions rose sharply while CTR fell, the AI Overview signature.
5. Confounders: no other edit, no update in the window, site-wide flat.
6. Verdict: **Helped (confidence: medium).** The page gained impressions and position on the target questions, and the impression-up, CTR-down pattern on one query suggests it is now appearing in an AI answer, though that is an inference, not a confirmed citation. Recommendation: keep it, pair with a citation tracker to confirm the AI Overview pickup, and apply the same quick-answer pattern to the sibling guides.
