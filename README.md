# Page Pulse Skills

Claude skills that measure and act on real SEO change data, powered by [Page Pulse](https://pagepulse.dev).

Most SEO tools grade a snapshot. Page Pulse records what changed on a page and when, and ties it to Google Search Console and GA4 outcomes, so these skills can answer the question other tools dodge: **did the change actually work?**

## Skills

| Skill | What it does |
| --- | --- |
| [change-autopsy](change-autopsy/) | Tells you whether a change to a page helped, hurt, or did nothing, with the evidence, then recommends keep, iterate, or revert. |
| [competitor-watch](competitor-watch/) | Watches competitor pages for real changes, alerts you by email or Slack, and summarizes what they changed so you can respond. |
| [geo-measurement-loop](geo-measurement-loop/) | Measures whether content you optimized for AI answers and AI Overviews actually moved your search outcomes. |
| [traffic-drop-detective](traffic-drop-detective/) | Diagnoses why a page lost traffic, separating a change you made from an external cause, then tells you to revert, fix, or wait. |
| [pre-deploy-seo-guard](pre-deploy-seo-guard/) | Checks what a page is already winning before you ship an edit, so you don't overwrite a ranking page — with the safeguard spelled out. |
| [quick-win-finder](quick-win-finder/) | Finds the handful of highest-leverage SEO moves available right now — striking-distance queries, CTR gaps, recoverable slips. |
| [content-brief](content-brief/) | Builds a writer-ready content brief grounded in a page's real Search Console query data, not guessed keywords. |

## Requirements

A connected **Page Pulse** account and its MCP. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), then add the Page Pulse MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

### Option A: Claude Code marketplace (one command)

In a Claude Code session:

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install change-autopsy@pagepulse-skills
/plugin install competitor-watch@pagepulse-skills
/plugin install geo-measurement-loop@pagepulse-skills
/plugin install traffic-drop-detective@pagepulse-skills
/plugin install pre-deploy-seo-guard@pagepulse-skills
/plugin install quick-win-finder@pagepulse-skills
/plugin install content-brief@pagepulse-skills
```

### Option B: copy the skill folder

Copy any skill folder (for example `change-autopsy/`) into your skills directory (`~/.claude/skills/` for Claude Code), then connect the Page Pulse MCP.

## Honesty

These skills treat correlation as correlation. They name what they ruled out and what they could not, never invent numbers, and say "too early to tell" when a window is too short. On a crawl-only Page Pulse project they confirm a change but will not fake a traffic verdict.

## License

MIT. See [LICENSE](LICENSE).
