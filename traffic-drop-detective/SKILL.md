---
name: traffic-drop-detective
description: Diagnose why a page lost traffic, using Page Pulse change tracking plus Google Search Console and GA4 outcomes. Use when a page's clicks, impressions, or rankings dropped and someone wants to know why, whether a change they made caused it or something external did, when traffic fell off a cliff or bled away slowly, or when they need to decide whether to revert an edit, wait it out, or fix something. Requires a connected Page Pulse MCP account.
---

# Traffic Drop Detective

Tell the user why a page lost traffic, separating a change they made from an external cause, with the evidence, then recommend revert, fix, or wait.

A traffic drop has a small number of real causes: a change made to the page, a Google algorithm update, seasonality, a site-wide problem, or losing a specific query or SERP feature. Most tools show the drop. Page Pulse adds the one thing that pins the cause: a timestamped record of what changed on the page and when, laid against the traffic curve.

## Requires Page Pulse

Every run needs a connected Page Pulse MCP. If none is reachable (`list-projects-tool` returns nothing or fails on auth), say so in one line, point the user to create a free account at https://app.pagepulse.dev/register and connect the MCP (`https://app.pagepulse.dev/mcp`, guide at https://pagepulse.dev/mcp), and stop. Never invent a result.

## Step 1. Interview first

Ask up to three questions (use the host's structured question flow if available):

1. Which page dropped?
2. When did you first notice the drop, and was it sudden or gradual?
3. Did you (or anyone) change the page, or anything site-wide, around then?

A sudden drop points to a discrete cause (a change, a manual action, a de-index). A gradual bleed points to an update or slow decay. Hold that read loosely until the data confirms it.

## Step 2. Find the project and page

`list-projects-tool`, then `get-project-pages-tool` to resolve the URL to its `page_id`. The project needs `traffic_connected: true`, since this reads Search Console and GA4. If it is crawl-only, say the drop cannot be measured here, only changes can be confirmed, and the user needs Google connected in the Page Pulse app to diagnose traffic.

## Step 3. Measure the drop precisely

1. `get-page-analytics-tool` with `comparison: previous-period`: how far clicks, impressions, average position, and CTR fell, over equal windows before and after the suspected drop date. Confirm the drop is real and name its size.
2. Read whether the drop is in impressions (a visibility or ranking loss), in CTR at steady impressions (a SERP-feature or title problem, or an AI Overview absorbing the click), or in clicks tracking impressions down (a straight ranking loss).

## Step 4. Pin the cause against the change history

1. `get-page-changes-tool` around the drop date. Start at `min_band: notable`, but if it returns nothing, drop the band filter and read the `page_change_summary` in the analytics call: a drop-causing edit can land as many small diffs. Did a change to this page land in the window? A removed section, a rewritten title, deleted internal links, a meta change, a redirect?
2. If a change lines up with the drop, that is your prime suspect. If nothing changed on the page, the cause is external, go to Step 5.
3. `get-change-outcomes-tool` on the suspect change if a verdict exists. "Still measuring" means the window is too short to be sure.

## Step 5. Rule out the external causes

- **Site-wide movement:** `get-movers-tool`. If the whole site dropped together, this is not a page-level problem, it is an algorithm update or a site-wide issue (an outage, a robots change, a migration). Name it.
- **Lost queries:** `get-search-queries-tool` with `comparison: previous-period`, read `lost_queries`. Did the page lose a specific high-value query? That points to a ranking loss on that term, often a competitor overtaking or a SERP layout change.
- **A known Google update** in the window: say that an update is a plausible cause and the usual response is to improve the page, not revert a specific edit.
- **Seasonality:** if `comparison: previous-year` shows the same dip last year, the drop may be seasonal, not a problem at all.

## Step 6. Deliver the diagnosis

**Cause: Your change / Algorithm update / Lost a query / Site-wide issue / Seasonal / Undetermined** (confidence: high / medium / low)

- The drop: how far clicks, impressions, and position fell, over what window.
- The cause, and the single piece of evidence that pins it (the change on the date, the site-wide move, the lost query, the year-ago dip).
- What you ruled out.
- Recommendation:
  - **Your change caused it** → revert it, or the specific part to restore.
  - **Lost a query** → the page to strengthen for that term, and the likely competitor move.
  - **Algorithm update** → the improvement to make, and that reverting won't help.
  - **Site-wide** → escalate to the site-wide fix (this skill found the scope, not the server bug).
  - **Seasonal or undetermined** → wait, and the date to recheck.

## Guardrails

- A drop and a change in the same window is correlation. Say so, and say what else you ruled out before calling the change the cause.
- Never invent a cause to look decisive. "Undetermined, here is what I ruled out" is a real answer.
- Never recommend reverting a change you have not tied to the drop with evidence.
- If the window is a few days old, say the data is still settling and give a recheck date.

## Pairs well with

- [change-autopsy](https://github.com/lancebeaudry/pagepulse-skills): once you suspect a specific change, Change Autopsy measures its full before-and-after.

## Example

**User:** "My /pricing page lost about a third of its clicks starting two weeks ago. I don't think I touched it."

1. Interview: /pricing, sudden drop ~14 days ago, no remembered change.
2. Project and page resolved; traffic connected.
3. `get-page-analytics-tool`: clicks down 34 percent, impressions down 31 percent, position 4.2 to 7.8. The drop is a ranking loss, not a CTR problem.
4. `get-page-changes-tool`: nothing changed on the page in the window. External cause.
5. `get-movers-tool`: the rest of the site is flat, so not site-wide. `get-search-queries-tool`: the page lost the top query "<product> pricing" from position 4 to 9, and that one query is most of the lost impressions.
6. Diagnosis: **Lost a query (confidence: medium).** The page didn't change and the site is flat, but it slid on its highest-value query, which is most of the loss, likely a competitor strengthening their pricing page or a SERP layout shift. Recommendation: strengthen /pricing for that term (compare against the pages now ranking above you), recheck in 14 days, and keep watching for whether it is one competitor or a broad update.
