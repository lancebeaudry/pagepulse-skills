# Traffic Drop Detective

A Claude skill that diagnoses why a page lost traffic and tells you what to do about it. Powered by [Page Pulse](https://pagepulse.dev).

A traffic drop has a few real causes: a change you made, a Google update, seasonality, a site-wide problem, or losing a specific query. Most tools show the drop. Page Pulse adds the record of what changed on the page and when, laid against the traffic curve, so the skill can separate a change you made from an external cause instead of guessing.

## What it does

- Measures the drop precisely (clicks, impressions, position, CTR, before and after).
- Reads whether it's a ranking loss, a CTR loss, or a visibility loss.
- Pins the cause against the page's change history and rules out the external ones (site-wide movement, lost queries, seasonality).
- Gives a diagnosis with a confidence level and a specific next step: revert, fix, or wait.

## Requirements

A connected **Page Pulse** account and its MCP, with Google connected for the Search Console and GA4 data. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), add the MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude, and connect Google in the Page Pulse app. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install traffic-drop-detective@pagepulse-skills
```

Or copy the `traffic-drop-detective/` folder into `~/.claude/skills/`.

## Pairs well with

[change-autopsy](https://github.com/lancebeaudry/pagepulse-skills) — once you suspect a specific change, it measures the full before-and-after.

## License

MIT. See the repository [LICENSE](../LICENSE).
