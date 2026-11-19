# Pre-Deploy SEO Guard

A Claude skill that checks what a page is already winning before you ship an edit to it, so you don't overwrite a page that's ranking. Powered by [Page Pulse](https://pagepulse.dev).

The most expensive SEO mistakes are self-inflicted: trimming the section that ranks, rewriting the title that earns the click, changing a URL with no redirect. This skill runs before the deploy, not the autopsy after. It reads the page's current Search Console standing and its recent change history, then tells you what the planned edit puts at risk and how to protect it.

## What it does

- Shows what the page is winning now (traffic and the exact queries it ranks for).
- Reads its recent change history so you don't pile an edit on one that's still settling.
- Maps your planned edit (new title, trimmed content, new URL, template change) onto the specific risk.
- Gives a ship / ship-with-a-safeguard / hold call, with the safeguard spelled out.

## Requirements

A connected **Page Pulse** account and its MCP, with Google connected for the Search Console data. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), add the MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude, and connect Google in the Page Pulse app. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install pre-deploy-seo-guard@pagepulse-skills
```

Or copy the `pre-deploy-seo-guard/` folder into `~/.claude/skills/`.

## Pairs well with

[change-autopsy](https://github.com/lancebeaudry/pagepulse-skills) — run it after the edit ships to confirm the thing you protected held. And any content or copy skill, which this tells which terms and sections to preserve.

## License

MIT. See the repository [LICENSE](../LICENSE).
