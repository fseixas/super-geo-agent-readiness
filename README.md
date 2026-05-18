# super-geo-agent-readiness

A Claude Skill that covers Generative Engine Optimization (GEO) and agent readiness in one place. SEO targeted blue links. This skill targets the answers AI engines produce, and the agents that fetch and act on your content.

## What it covers

Four optimization surfaces, one routing layer:

- **Content.** Authority, quotability, comprehensiveness, structure. The patterns that get a page cited by ChatGPT, Perplexity, Claude, and Google AI Overviews.
- **Technical site.** FAST framework, Schema.org JSON-LD, robots.txt for AI crawlers, `llms.txt` and `llms-full.txt` at small-site, large-site, and per-directory scales.
- **Platform tactics.** Per-engine optimization for ChatGPT, Perplexity, Google AI Overviews, AI Mode, Claude, Gemini, Copilot, and Grok, with current traffic-share numbers so you know where to spend effort.
- **Agent readiness.** MCP Server Cards, A2A Agent Cards, OpenAPI, API Catalog (RFC 9727), OAuth metadata (RFC 8414/9728), Web Bot Auth, x402, ACP, UCP, Markdown content negotiation. Compiled from Cloudflare's agent-readiness work and the agentready.org open specification.

## What's inside

```
super-geo-agent-readiness/
├── SKILL.md                              Router. Picks the right reference for the task.
└── references/
    ├── content-strategy.md               Four pillars, chunkability, E-E-A-T, pre-publish checklist.
    ├── technical-implementation.md       FAST framework, Core Web Vitals, semantic HTML.
    ├── structured-data.md                JSON-LD for Article, FAQ, Organization, Product, HowTo, Person.
    ├── ai-crawlers-and-llmstxt.md        Crawler list, robots.txt, llms.txt formats.
    ├── platforms.md                      Per-engine optimization tactics.
    ├── agent-readiness.md                MCP, OAuth, x402, A2A, API Catalog.
    ├── measurement.md                    Benchmarks, GA4 regex for AI referral traffic, monitoring tools.
    ├── audit-checklist.md                Severity-graded audit with a final report template.
    └── templates.md                      Every config in one file, ready to paste.
```

## When Claude triggers it

Any of these signals fire the skill: GEO, AEO, LLMO, AI SEO, "rank in ChatGPT", "show up in Perplexity", "appear in AI Overviews", `llms.txt`, agent readiness, MCP server discovery, Web Bot Auth, OAuth for agents, x402, or asking Claude to audit a site for AI visibility.

## Install

**Claude.ai:** download `super-geo-agent-readiness.skill` from Releases and upload it under Settings → Capabilities → Skills.

**Claude Code:** clone this repo into your project's `.claude/skills/` directory, or into `~/.claude/skills/` for global use.

## Sources

Compiled and extended from:

- [awesome-geo](https://github.com/luka2chat/awesome-geo)
- [geo-skills](https://github.com/luka2chat/geo-skills)
- [seo-geo-claude-skills](https://github.com/aaron-he-zhu/seo-geo-claude-skills)
- Cloudflare, [Agent Readiness](https://blog.cloudflare.com/agent-readiness/)
- [agentready.org](https://agentready.org) open specification

## License

MIT
