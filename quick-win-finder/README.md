# Quick Win Finder

A Claude skill that finds the handful of highest-leverage SEO moves available right now and tells you exactly what to change. Powered by [Page Pulse](https://pagepulse.dev).

Most SEO advice is a backlog of a hundred generic fixes. This finds the few that move the needle fastest: pages sitting just off page one, pages with impressions but a weak click rate, and pages that recently slipped and can be recovered. Every opportunity is grounded in your own Search Console data, so it's a real win, not a checklist item.

## What it does

- Pulls three opportunity types: striking-distance queries (positions ~5–15), high-impression / low-CTR pages, and recent slippers.
- Qualifies each against real volume and the page's change history (a slip is often a self-inflicted edit to revert — the fastest win of all).
- Returns a short, ranked list: the page, the query, the specific move, and rough effort.
- Ends with a single "if you do one thing" pick.

## Requirements

A connected **Page Pulse** account and its MCP, with Google connected for the Search Console data. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), add the MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude, and connect Google in the Page Pulse app. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install quick-win-finder@pagepulse-skills
```

Or copy the `quick-win-finder/` folder into `~/.claude/skills/`.

## Pairs well with

[change-autopsy](https://github.com/lancebeaudry/pagepulse-skills) — after you make one of these edits, measure whether it worked. And a content or copy skill, which you hand the striking-distance query to build around.

## License

MIT. See the repository [LICENSE](../LICENSE).
