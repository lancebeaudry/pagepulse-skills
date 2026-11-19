---
name: content-brief
description: Build a content brief grounded in a page's real Google Search Console query data, using Page Pulse, so a writer targets the terms the page already earns impressions for instead of guessed keywords. Use when someone wants a brief or outline for a new or existing page, wants to know what to write about or which questions to answer, is about to hand a page to a writer, or wants their content plan grounded in real search demand rather than a keyword tool's guesses. Requires a connected Page Pulse MCP account.
---

# Content Brief

Build a writer-ready content brief for a page, grounded in the real queries it already earns impressions for and the terms it is closest to winning, so the content targets demonstrated demand instead of guessed keywords.

Most briefs start from a keyword tool's estimate of what people might search. For a page that already exists, or a site with a related page, you have something better: the actual queries Google is showing the page for, the ones it is close on, and the ones it recently lost. Page Pulse surfaces that Search Console data, so the brief is built on what the site is already proving it can rank for.

## Requires Page Pulse

Every run needs a connected Page Pulse MCP. If none is reachable (`list-projects-tool` returns nothing or fails on auth), say so in one line, point the user to create a free account at https://app.pagepulse.dev/register and connect the MCP (`https://app.pagepulse.dev/mcp`, guide at https://pagepulse.dev/mcp), and stop. Never invent a result.

## Step 1. Interview first

Ask up to three questions (use the host's structured question flow if available):

1. What page is the brief for? (an existing URL to improve, or a planned page and its closest existing sibling)
2. What's the goal of the page? (rank for a topic, convert a buyer, answer a question, support a product)
3. Who's writing it, and how much do they know the subject? (shapes how much context the brief carries)

## Step 2. Find the project and page

`list-projects-tool`, then `get-project-pages-tool`. For an existing page, resolve its `page_id`. For a planned page, resolve the closest existing sibling to borrow its query data. The project needs `traffic_connected: true`. If it is crawl-only, say the brief can't be grounded in search data without Google connected, and offer a thinner topic-based brief instead.

## Step 3. Pull the real query data

1. `get-search-queries-tool` on the page (or sibling), recent window: the queries it earns impressions for, with position and clicks. This is the demand the page is already touching.
2. Read it for three things:
   - **Winning queries** (good position, real clicks): the core the page must keep serving.
   - **Striking-distance queries** (positions ~5–15, impressions but few clicks): the terms a stronger page could capture. These are the brief's biggest opportunity.
   - **New and lost queries** (`new_queries` / `lost_queries`): rising demand to lean into, and ground recently ceded to recover.
3. `get-page-analytics-tool` for the page's overall standing, so the brief knows whether it is strengthening a winner or rebuilding a slipper.

## Step 4. Shape the brief around the data

Turn the queries into an outline, not a keyword list:

- **Primary intent**: the one job the page does, drawn from its strongest queries.
- **Sections to cover**: each built around a cluster of real queries (especially the striking-distance ones), phrased as the questions searchers are actually asking.
- **Questions to answer explicitly**: the question-shaped queries, verbatim, so the page can be cited and can win featured snippets and AI answers.
- **Terms to keep**: on an existing page, the language tied to its winning queries, so the rewrite doesn't drop what already ranks (hand this to the Pre-Deploy SEO Guard if the page is live and strong).
- **What to add**: the gap between what the page covers and what its striking-distance and new queries demand.

## Step 5. Deliver the brief

A clean, writer-ready brief:

- **Page and goal**, one line.
- **Primary query and intent.**
- **Outline**: sections, each with the real queries it should serve and the questions to answer.
- **Must-keep terms** (for an existing page).
- **Opportunity call-out**: the one or two striking-distance queries this brief is really chasing, and why they're winnable.
- **Length and depth guidance**, matched to what currently ranks for the primary query, not a generic word count.

Keep it the kind of document a writer can work straight from. No filler, no invented statistics.

## Guardrails

- Every section and term traces to a real query from the data. Don't pad the brief with keywords the site has no signal for.
- Don't invent search volumes or difficulty scores. Page Pulse gives impressions, positions, and clicks, use those.
- On an existing strong page, flag what to preserve as loudly as what to add.
- If the page or sibling has thin query data, say so, and make the brief honest about how much is grounded versus inferred.

## Pairs well with

- A content writing or copy skill: it writes, this tells it exactly what to write toward.
- [pre-deploy-seo-guard](https://github.com/lancebeaudry/pagepulse-skills): before a rewrite of a live page, confirm which terms to protect.
- [change-autopsy](https://github.com/lancebeaudry/pagepulse-skills): after the new content ships, measure whether it won the queries the brief targeted.

## Example

**User:** "Brief for a rewrite of /guides/email-deliverability. A freelance writer who knows email will do it."

1. Interview: existing page, goal is to rank and get cited for deliverability questions, writer knows the subject.
2. Project and page resolved; traffic connected.
3. `get-search-queries-tool`: the page wins "email deliverability" (#4) and "improve deliverability" (#5). Striking distance: "why do my emails go to spam" (#8, 2,100 impressions, few clicks) and "email deliverability checklist" (#11). New query rising: "deliverability for cold email". Lost: "email bounce rate" fell off.
4. Shape: primary intent is a definitive deliverability guide. Sections built on the striking-distance queries, with "why do my emails go to spam" answered as an explicit, citable section near the top. Keep the language tied to the two winning queries. Add a checklist section (there's demand and the page lacks one) and a cold-email passage.
5. Brief delivered: goal line, primary query "email deliverability", an outline whose spam-folder and checklist sections target the two striking-distance queries verbatim, a must-keep list (the phrasing that holds #4 and #5), the opportunity call-out ("why do my emails go to spam" — 2,100 impressions at position 8 is the single biggest winnable gain), and depth guidance matched to the current page-one results. Run Change Autopsy a few weeks after it ships to confirm it captured the spam-folder query.
