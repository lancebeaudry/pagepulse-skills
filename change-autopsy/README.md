# Change Autopsy

A Claude skill that tells you whether a change you made to a web page helped, hurt, or did nothing, with the evidence, then recommends whether to keep it, iterate on it, or revert it.

Most SEO tools grade a page as it is today. Change Autopsy answers the question they dodge: *did the thing I changed actually work?* It is built on [Page Pulse](https://pagepulse.dev), which records what changed on a page and when, and ties it to Search Console and GA4 outcomes.

## What it does

- Interviews you briefly: which page, roughly when it changed, and what you were trying to improve.
- Pins the exact change from Page Pulse's history (and catches full rewrites that split into many small diffs).
- Measures a post-change window against an equal pre-change window: clicks, impressions, position, CTR, conversions, and query-level movement.
- Rules out the confounders (Google updates, other edits, site-wide movement, too-short windows) before it calls a verdict.
- Returns a one-line verdict with a confidence level and a concrete next step.

## Requirements

A connected **Page Pulse** account and its MCP. The skill reads your projects, page-change history, and traffic through the Page Pulse MCP tools. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), then add the Page Pulse MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

Copy the `change-autopsy` folder into your skills directory (for Claude Code: `~/.claude/skills/`), then connect the Page Pulse MCP. Ask Claude something like "did my edit to /pricing work?" and the skill triggers.

## Honesty

Correlation is not proof. The skill names what it ruled out and what it could not, never invents numbers, and says "too early to tell" when the window is too short. On a crawl-only Page Pulse project it confirms the change but will not fake a traffic verdict.

## License

MIT. See [LICENSE](LICENSE).
