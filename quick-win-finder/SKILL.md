---
name: quick-win-finder
description: Find the pages and queries with the most upside right now, using Page Pulse plus Google Search Console, then tell the user what to change on each. Use when someone asks what to work on next, where the easy SEO wins are, which pages are close to page one, where they're leaving clicks on the table, or wants a prioritized list of high-leverage edits instead of a full audit. Requires a connected Page Pulse MCP account.
---

# Quick Win Finder

Give the user a short, ranked list of the highest-leverage SEO moves available right now, each tied to a real page and a real query, with the specific edit to make.

Most SEO advice is a backlog of a hundred generic fixes. This finds the handful that move the needle fastest: pages sitting just off page one, pages with impressions but a weak click rate, and pages that recently slipped and can be recovered. Each is grounded in the user's own Search Console data, so it is a real opportunity, not a checklist item.

## Requires Page Pulse

Every run needs a connected Page Pulse MCP. If none is reachable (`list-projects-tool` returns nothing or fails on auth), say so in one line, point the user to create a free account at https://app.pagepulse.dev/register and connect the MCP (`https://app.pagepulse.dev/mcp`, guide at https://pagepulse.dev/mcp), and stop. Never invent a result.

## Step 1. Interview first

Ask up to three questions (use the host's structured question flow if available):

1. Which site or project should I look at?
2. Anything to focus on or avoid? (a section, a product line, pages you can't touch right now)
3. How much appetite do you have? (one or two quick edits, or a prioritized list to work through)

If the user just wants "the top few", keep the output to three.

## Step 2. Find the project

`list-projects-tool`, resolve the project. It needs `traffic_connected: true`, since every opportunity type here is drawn from Search Console. If it is crawl-only, say quick wins can't be found without Google connected, and stop.

## Step 3. Pull the three opportunity types

1. **Striking distance.** `get-project-analytics-tool` / `get-search-queries-tool` at project scope to find queries ranking in positions roughly 5 to 15: close enough that a focused edit can push them onto page one or up it. These are the highest-ROI moves.
2. **High impressions, low CTR.** Pages or queries with real impressions but a click rate below what their position should earn. The fix is usually a title and meta rewrite, cheap and fast, no ranking change required.
3. **Recent slippers.** `get-movers-tool` for pages that dropped recently. A page that was winning and slipped is often easier to recover than a cold page is to rank, especially if `get-page-changes-tool` shows a recent edit that may have caused it.

## Step 4. Qualify each candidate

For the top handful, before you recommend it:

- Confirm the query actually has volume worth chasing (a position-6 query with three impressions a month is not a win).
- Check `get-page-changes-tool` on the page: if it changed recently, factor that in (a slip may be a self-inflicted edit to revert, which is the fastest win of all).
- Make sure the opportunity is actionable: the user owns the page and can edit it.

## Step 5. Deliver the ranked list

A short table or list, highest leverage first. For each:

- **The page and the query**, with its current position and impressions.
- **The opportunity type** (striking distance / CTR gap / recoverable slip).
- **The specific move**: "push position 7 to page one by strengthening the section on <query>", "rewrite the title to earn the clicks these impressions should", "revert the <date> edit that dropped this page".
- **Rough effort**: a title rewrite is minutes, a content build is an afternoon. Rank quick-and-high first.

Close with a one-line "if you do one thing" pick.

## Guardrails

- Rank by leverage, not by count. Three real wins beat twenty filler items.
- Every opportunity must trace to a real query with real impressions. Never pad the list.
- Don't recommend chasing a query the page can't realistically win, or one with no volume.
- If there genuinely aren't easy wins (a healthy, mature site), say so, and point at the one or two harder plays instead of inventing easy ones.

## Pairs well with

- [change-autopsy](https://github.com/lancebeaudry/pagepulse-skills): after you make one of these edits, measure whether it worked.
- A content brief or copy skill: hand it the striking-distance query to build or rewrite around.

## Example

**User:** "What should I work on next on the blog? Give me the top three."

1. Interview: the blog project, no exclusions, top three.
2. Project resolved; traffic connected.
3. Striking distance: "<topic> checklist" ranks position 6 with 1,900 impressions a month. CTR gap: a how-to post gets 4,000 impressions at a 1.1 percent click rate, low for its position 5. Recent slipper: a comparison post fell from position 4 to 9 three weeks ago.
4. Qualify: the checklist query has real volume and the page is thin on it. The how-to's title is generic. The comparison post's slip lines up with a `get-page-changes-tool` edit on the same date.
5. The three:
   1. **Comparison post, revert the recent edit.** It fell from 4 to 9 right when it was edited three weeks ago. Fastest, highest-confidence win: restore what dropped it. (Minutes.)
   2. **"<topic> checklist", position 6 → page one.** 1,900 impressions sitting just off page one; expand the checklist section and add the specifics the top results cover. (An afternoon.)
   3. **How-to post, rewrite the title.** 4,000 impressions at 1.1 percent CTR is leaving clicks on the table; a sharper title earns them with no ranking change. (Minutes.)

   If you do one thing: revert the comparison-post edit. You're recovering a page that was already winning.
