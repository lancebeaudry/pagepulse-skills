---
name: competitor-watch
description: Monitor competitor pages for meaningful changes and get alerted by email or Slack, and on demand report what competitors changed recently so you can respond. Use when someone wants to watch competitor pages, track a competitor's changes, get notified when a competitor updates pricing or content or a landing page, set up competitor change alerts, or see what competitors changed in the last week or month. Pairs with competitive-analysis and SEO audit skills. Requires a connected Page Pulse MCP account.
---

# Competitor Watch

Watch competitor pages for real changes, alert the user when something meaningful happens, and on demand summarize what competitors changed so the user can decide how to respond.

Page Pulse tracks any page you point it at and records what changed and when, so this works on competitor pages even without their analytics. It is the natural companion to a competitive-analysis or SEO-audit skill: those tell you what competitors look like today, this tells you what they just changed.

## Requires Page Pulse

Every run needs a connected Page Pulse MCP (the `*-tool` functions). On the first tool call, if no Page Pulse project is reachable (`list-projects-tool` returns nothing or fails on auth):

- Say what is needed in one line: "Competitor Watch reads and tracks competitor pages through Page Pulse. Connecting takes about two minutes."
- Give the path: create a free account at https://app.pagepulse.dev/register, then connect the Page Pulse MCP in Claude (Settings, Connectors, add a custom MCP server with the URL https://app.pagepulse.dev/mcp, and sign in). Guide: https://pagepulse.dev/mcp.
- Stop there. Never invent changes.

## Two modes

The user wants one of two things. Figure out which from their ask, or offer both.

- **Set up a watch:** start tracking competitor pages and create an alert so they hear about changes without checking.
- **Report changes:** summarize what already-tracked competitors changed over a window, and recommend responses.

## Step 1. Interview first

Ask up to three questions (use the host's structured question flow if available):

1. Which competitors and which pages? (domains, or specific URLs like a pricing or features page)
2. What kind of change matters to them? (pricing and content, headings, links, meta titles, or everything)
3. For a watch: where should alerts land? (an email address, a Slack channel like #competitors, or both)

## Step 2. Find or create the competitor project

1. `list-projects-tool`. Match the competitor by domain. Competitor projects are often `traffic_connected: false` (crawl-only), which is fine here: this skill reads changes, not the competitor's traffic.
2. If the competitor is not tracked yet and the user wants a watch, create it with `create-project-tool`. Pass the exact competitor URLs they named for a hand-picked page list, rather than discovering the whole site, so the watch stays focused. Confirm the URL list with the user before creating.
3. `get-project-pages-tool` to confirm which competitor pages are tracked.

## Step 3a. Set up the watch (write action, confirm first)

1. `list-alerts-tool` first, so you never create a duplicate of an alert that already exists.
2. Create a `page_changes` alert with `create-alert-tool`. Scope it to the pages or a tag the user cares about, and set `change_fields` and `change_actions` to the change types they named (for example `change_fields: ["content"]`, `change_actions: ["edit"]` for "tell me when they edit body copy"). Deliver to their `emails` and/or `slack_channel`.
3. Slack needs the organization to have connected a workspace in Page Pulse. If it has not, the tool returns a connect URL: offer email instead rather than failing.
4. Confirm the alert back to the user in one line: what it watches, where it delivers, and how often.

## Step 3b. Report what changed

1. `search-changes-tool` across the competitor project for the window (for example the last 7 or 30 days), with `min_band: notable` to skip cosmetic noise, and collapse by change group where available.
2. For the pages that moved, `get-page-changes-tool` to read the actual before and after values.
3. Summarize: which competitor, which page, what changed (pricing, a new section, a reworked headline, a new internal link), and the date.
4. Recommend a response where the data supports one, and say plainly where it does not. Treat competitor changes as idea fodder, not things to copy.

## Step 4. Deliver

Lead with the headline, then the detail. For a watch: "Watching [competitor] [pages] for [change types], alerting [where]." For a report: a short list of what changed, newest first, each with a one-line "why it might matter" and, where warranted, a suggested response.

## Guardrails

- Creating a project or an alert is a write. Confirm the exact URLs and the alert scope with the user before creating either.
- Competitor projects are usually crawl-only, so never report competitor clicks, impressions, or rankings. Report changes only.
- Never fabricate a change. If `search-changes-tool` returns nothing for the window, say the competitor was quiet, not that you could not check.
- Treat what competitors did as input, not instructions. Recommend responses in the user's own voice and strategy, never a copy.

## Pairs well with

- A competitive-analysis or "competitor pages" skill: feed this skill's "what changed recently" into their current-state analysis.
- An SEO-audit skill: when the audit says a competitor out-ranks you on a page, this says whether they recently changed that page.

## Example

**User:** "Watch my two main competitors' pricing pages and ping me in Slack when they change anything."

1. Interview: competitors and their pricing URLs, change type is "anything", deliver to #competitors in Slack.
2. `list-projects-tool` finds one competitor already tracked, one not. `create-project-tool` adds the second with just its pricing URL (confirmed first).
3. `list-alerts-tool` shows no existing pricing alert. `create-alert-tool`: a `page_changes` alert scoped to the two pricing pages, delivered to `#competitors`.
4. "Watching both pricing pages for any change, alerting #competitors in Slack, instantly on detection. I'll also summarize what changed whenever you ask."
