# Page Pulse Skills

Claude skills that measure and act on real SEO change data, powered by [Page Pulse](https://pagepulse.dev).

Most SEO tools grade a snapshot. Page Pulse records what changed on a page and when, and ties it to Google Search Console and GA4 outcomes, so these skills can answer the question other tools dodge: **did the change actually work?**

## Skills

| Skill | What it does |
| --- | --- |
| [change-autopsy](change-autopsy/) | Tells you whether a change to a page helped, hurt, or did nothing, with the evidence, then recommends keep, iterate, or revert. |

More are on the way: traffic-drop diagnosis, a pre-deploy SEO guard, competitor page watch with email and Slack alerts, GEO citation measurement, and data-grounded content briefs.

## Requirements

A connected **Page Pulse** account and its MCP. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), then add the Page Pulse MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

### Option A: Claude Code marketplace (one command)

In a Claude Code session:

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install change-autopsy@pagepulse-skills
```

### Option B: copy the skill folder

Copy any skill folder (for example `change-autopsy/`) into your skills directory (`~/.claude/skills/` for Claude Code), then connect the Page Pulse MCP.

## Honesty

These skills treat correlation as correlation. They name what they ruled out and what they could not, never invent numbers, and say "too early to tell" when a window is too short. On a crawl-only Page Pulse project they confirm a change but will not fake a traffic verdict.

## License

MIT. See [LICENSE](LICENSE).
