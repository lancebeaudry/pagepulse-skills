# Competitor Watch

A Claude skill that watches competitor pages for real changes, alerts you by email or Slack when something meaningful happens, and summarizes what competitors changed so you can decide how to respond. Powered by [Page Pulse](https://pagepulse.dev).

Page Pulse tracks any page you point it at, including competitor pages, and records what changed and when. So this works without a competitor's analytics: it reports changes, not their traffic.

## What it does

- Sets up a focused watch on the competitor pages you care about and an alert that reaches you by email, Slack, or both.
- On demand, summarizes what competitors changed over a window (pricing, content, headings, links, meta) newest first.
- Recommends responses where the data supports one, and treats competitor changes as idea fodder, not a script to copy.

## Requirements

A connected **Page Pulse** account and its MCP. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), then add the Page Pulse MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp). Slack delivery needs a Slack workspace connected in the Page Pulse app.

## Install

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install competitor-watch@pagepulse-skills
```

Or copy the `competitor-watch/` folder into `~/.claude/skills/`.

## Pairs well with

A competitive-analysis or SEO-audit skill (for example the competitor-pages workflows in [claude-seo](https://github.com/AgriciDaniel/claude-seo)). Those describe what competitors look like today; this tells you what they just changed.

## License

MIT. See the repository [LICENSE](../LICENSE).
