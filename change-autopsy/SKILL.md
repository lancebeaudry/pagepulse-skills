---
name: change-autopsy
description: Measure whether a specific change to a web page helped, hurt, or did nothing, using Page Pulse change tracking combined with Google Search Console and GA4 outcomes. Use when someone asks "did my edit work", "did that change help or hurt", wants a before and after on a page they updated, wants to know whether a title, content, or meta change moved rankings, clicks, or conversions, or wants to decide whether to keep, iterate on, or revert a change. Requires a connected Page Pulse MCP account.
---

# Change Autopsy

Tell the user, with evidence, whether a change they made to a page helped, hurt, or did nothing, then recommend whether to keep it, iterate on it, or revert it.

This skill is built on the one thing snapshot SEO tools cannot do: Page Pulse records what changed on a page and when, and ties it to Search Console and GA4 outcomes. Correlation is not proof, so the real job is to weigh the evidence honestly and say how confident you are.

## Requires Page Pulse

Every run needs a connected Page Pulse MCP (the `*-tool` functions). On the first tool call, if no Page Pulse project is reachable (`list-projects-tool` returns nothing or fails on auth):

- Say in one line what is needed and why: "Change Autopsy reads your page-change history and traffic from Page Pulse. Connecting takes about two minutes."
- Give the path: create a free account at https://app.pagepulse.dev/register, then connect the Page Pulse MCP in Claude. Open Settings, then Connectors (or Integrations), add a custom MCP server with the URL https://app.pagepulse.dev/mcp, and sign in when Page Pulse redirects. The full setup guide is at https://pagepulse.dev/mcp.
- Stop there. Never fabricate a verdict without data.

## Step 1. Interview first, before any tool call

Ask the user up to three questions (use the host's structured question flow if available). Do not ask for anything the data can answer, such as UUIDs or exact metrics.

1. Which page changed? (a URL, or a description you can match to a tracked page)
2. Roughly when did the change go live? (a date, or "about three weeks ago")
3. What were you trying to improve? (rankings for a query, clicks, conversions or signups, or "not sure, just tell me what moved")

The third answer sets the yardstick. "Did it work" is meaningless without the goal the user had in mind. If they name a specific query or event, measure that one first.

## Step 2. Find the project and the page

1. `list-projects-tool` to get projects. If more than one, match the page's domain; if still ambiguous, ask which project.
2. `get-project-pages-tool` to resolve the URL to the tracked page and its `page_id`. If the page is not tracked, say so plainly: Page Pulse can only autopsy a page it was already watching when the change happened.
3. Check the project's `traffic_connected` flag from `list-projects-tool`. If it is false, the project is crawl-only: you can pin and describe the change (Step 3) but cannot measure its traffic outcome. Say that plainly and stop at the change description, rather than implying a verdict you have no data for.

## Step 3. Pin down the actual change

1. `get-page-changes-tool` for that page around the stated date. Start with `min_band: notable`, but know the trap: a full rewrite usually lands as many small diffs that each score low or noise, so `notable` can return zero on a page that was completely rewritten. When it returns nothing, do not conclude the page did not change. Drop the band filter and read the raw diffs, and treat the `page_change_summary` block in `get-page-analytics-tool` (Step 4) as the authoritative "did something change on this date" signal: its per-date `max_impact_band` reflects the aggregate event (a rewrite shows there as critical even when no single field diff is notable). Identify the specific change: which fields moved (meta, headings, content, links) and the exact date.
2. If the user's memory of the date and the recorded change disagree, trust the record and say so.
3. `get-change-outcomes-tool` (by `page_id`). This is Page Pulse's own computed verdict once enough time has passed, and it is your strongest single signal when present. If it says the change is not evaluated yet, the change is too recent to judge on its own: lean on Step 4 and flag low confidence.

## Step 4. Measure the outcome

Compare a post-change window against an equal-length pre-change window.

1. `get-page-analytics-tool` with `comparison: previous-period` on the page: clicks, impressions, average position, CTR, and key events (conversions) if the project has GA4 connected. Use `view_scope: landing` when conversions are the goal.
2. `get-search-queries-tool` with `comparison: previous-period` for the page. The story usually lives here: which queries gained or lost position and clicks after the change.
3. If the goal was a specific query or event, report that line first and in plain numbers.

Window rules:

- Give the change at least 14 days of post-change data; 28 is better. GSC also lags two to three days.
- Compare equal-length windows, and watch for seasonality. Say so when the two windows are not comparable.

## Step 5. Rule out the confounders

Before calling it, check what else could explain the move:

- A known Google update inside the window. Say so if the move lines up with one.
- Other changes to the same page in the window (`get-page-changes-tool` across the full range) that muddy attribution.
- Site-wide movement: if everything moved the same way, this page's change is probably not the cause. `get-movers-tool` gives that context.
- Too little time: if the window is short, the honest answer is "too early to tell."

## Step 6. Deliver the verdict

Lead with the verdict in one line, then the evidence, then the recommendation. Use this shape:

**Verdict: Helped / Hurt / No measurable effect / Too early to tell** (confidence: high / medium / low)

- What changed: the fields, on the date.
- Goal: what the user wanted to improve.
- What happened: the key metric move, with before and after numbers and the window, then the query-level detail.
- Confounders: what you ruled out, and what you could not.
- Recommendation: keep it / iterate (with the specific next edit) / revert / wait and recheck on a named date.

Never state a move as caused by the change when you only have correlation. Say "clicks rose 22 percent in the 28 days after, with no other change on the page and no Google update in the window" and let that carry the weight.

## Guardrails

- Never invent numbers, dates, or a verdict. If the data is missing or thin, say so.
- Do not recommend reverting a change made in the last 14 days unless it clearly broke something (an error, a wrong redirect, a ranking cliff).
- One page per run unless the user asks to batch several.
- If the page is not tracked or the change is not recorded, stop and explain, rather than guessing from the live page alone.

## Example (a real run)

User: "Did the rewrite on /free-texts/ work?"

1. Interview: page /free-texts/, rewrite in early November, goal is clicks and visibility (no conversions tracked on this page).
2. `list-projects-tool` finds the project; `get-project-pages-tool` resolves /free-texts/ to its `page_id`.
3. `get-page-changes-tool` at `min_band: notable` returns nothing, but `get-page-analytics-tool`'s `page_change_summary` shows a critical-band, 21-field change on Nov 1. Dropping the band filter reveals a full rewrite: a quick-answer intro, a comparison table, new H2s, and jump-link nav. (This is the trap in Step 3: a rewrite split into small diffs can score zero at the notable band.)
4. `get-change-outcomes-tool` has no evaluated verdict yet (still inside the measurement horizon). `get-page-analytics-tool` (previous-period, organic): clicks 1,543 to 4,755 (+208 percent), CTR 2.87 to 4.82 percent, average position 10.2 to 8.9. `get-search-queries-tool`: the "free text online" query cluster sits at positions 6 to 9, with the query set broadening.
5. Confounders: only 16 days of post-change data; a site-wide migration is lifting many pages at once, so the volume gain is partly a rising tide. The 68 percent CTR jump is the page-specific signal, since a sitewide lift does not raise one page's CTR.
6. Verdict: **Helped (confidence: medium-high).** Clicks tripled and CTR rose 68 percent after the rewrite, with CTR isolating the edit from the migration. Recommendation: keep it, recheck `get-change-outcomes` after a full horizon, then roll the same pattern to the sibling pages that are also climbing.
