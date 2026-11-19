# Content Brief

A Claude skill that builds a writer-ready content brief grounded in a page's real Google Search Console query data, not guessed keywords. Powered by [Page Pulse](https://pagepulse.dev).

Most briefs start from a keyword tool's estimate of what people might search. For a page that already exists — or a site with a related page — you have something better: the queries Google already shows the page for, the ones it's closest to winning, and the ones it recently lost. This builds the brief on what the site is already proving it can rank for.

## What it does

- Pulls the page's real queries: what it wins, what it's in striking distance of, what's rising, what it lost.
- Turns them into an outline — sections and explicit questions to answer — not a keyword list.
- Flags the must-keep terms so a rewrite doesn't drop what already ranks.
- Calls out the one or two striking-distance queries the brief is really chasing, and sets depth against what currently ranks.

## Requirements

A connected **Page Pulse** account and its MCP, with Google connected for the Search Console data. Create a free account at [app.pagepulse.dev/register](https://app.pagepulse.dev/register), add the MCP server (`https://app.pagepulse.dev/mcp`) as a custom connector in Claude, and connect Google in the Page Pulse app. Setup guide: [pagepulse.dev/mcp](https://pagepulse.dev/mcp).

## Install

```
/plugin marketplace add lancebeaudry/pagepulse-skills
/plugin install content-brief@pagepulse-skills
```

Or copy the `content-brief/` folder into `~/.claude/skills/`.

## Pairs well with

A content writing or copy skill (it writes, this tells it what to write toward), [pre-deploy-seo-guard](https://github.com/lancebeaudry/pagepulse-skills) (confirm which terms to protect before a rewrite), and [change-autopsy](https://github.com/lancebeaudry/pagepulse-skills) (measure whether the new content won the queries the brief targeted).

## License

MIT. See the repository [LICENSE](../LICENSE).
