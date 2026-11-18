# GEO Measurement Loop

A Claude skill that measures whether content you optimized for AI answers and AI Overviews actually moved your search outcomes. Powered by [Page Pulse](https://pagepulse.dev).

The GEO and AEO world has a known hole: people optimize for AI citation and can't measure whether it worked. This closes the search half of that loop. Page Pulse records the GEO edit and ties it to Search Console, so you can see whether impressions, clicks, position, and CTR moved for your target queries.

## What it measures, and what it doesn't

- **Measures:** the search outcome of your GEO content edits, before and after, for the page and its target queries, including the AI Overview signature (impressions up, CTR down).
- **Does not measure:** a literal citation inside ChatGPT, Perplexity, or an AI Overview. That needs a dedicated citation tracker, which this pairs with for the other half.
- Page Pulse tracks content, headings, text, links, and meta. It does not track schema markup, so schema-only GEO work is not visible to it.

## Requirements

A connected **Page Pulse** account and its MCP, with Google connected for the Search Console data. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), add the MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude, and connect Google in the Page Pulse app. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install geo-measurement-loop@pagepulse-skills
```

Or copy the `geo-measurement-loop/` folder into `~/.claude/skills/`.

## Pairs well with

A GEO or AEO optimization skill such as [geo-seo-claude](https://github.com/zubair-trabzada/geo-seo-claude) (it optimizes, this measures), and an AI-citation tracker (it counts citations, this ties search movement to the edit).

## License

MIT. See the repository [LICENSE](../LICENSE).
