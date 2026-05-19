# Super GEO Agent Readiness

A Claude Skill that covers Generative Engine Optimization (GEO) and agent readiness in one place. SEO targeted blue links. This skill targets the answers AI engines produce, and the agents that fetch and act on your content.

## What it covers

Four optimization surfaces, one routing layer:

- **Content.** Authority, quotability, comprehensiveness, structure, primary-source signaling. The patterns that get a page cited by ChatGPT, Perplexity, Claude, and Google AI Overviews.
- **Technical site.** FAST framework, Schema.org JSON-LD, robots.txt for AI crawlers, `llms.txt` and `llms-full.txt` at small-site, large-site, and per-directory scales.
- **Platform tactics.** Per-engine optimization for ChatGPT, Perplexity, Google AI Overviews, AI Mode, Claude, Gemini, Copilot, and Grok, with current traffic-share numbers so you know where to spend effort.
- **Agent readiness.** MCP Server Cards, A2A Agent Cards, OpenAPI, API Catalog (RFC 9727), OAuth metadata (RFC 8414/9728), Web Bot Auth, x402, ACP, UCP, Markdown content negotiation. Compiled from Cloudflare's agent-readiness work and the agentready.org open specification.

## Per-engine calibration: policy vs engineering

Google publishes its own [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) and classifies several common GEO tactics as "myths" for Google AI features. The skill treats Google's guide as a policy document, not as engineering reality. Concrete signals undercut the "myths" framing: Google's own Lighthouse tool checks for `llms.txt`, two senior Googlers (John Mueller and Addy Osmani) have publicly given opposite advice on markdown pages, Anthropic's published Claude system prompt contains explicit source-quality filters that target SEO-pattern content, and modern retrieval architecture operates on chunks regardless of what the policy document says.

Operational conclusion: for Google AI Overviews and AI Mode, classic SEO at peak quality is the foundation. For ChatGPT, Perplexity, Claude, and training corpora, the additional surfaces in this skill apply. See `references/content-strategy.md` for the three-layer arbitration model (latent knowledge / active retrieval / arbitration) that explains the mechanism.

## What's inside

```
super-geo-agent-readiness/
├── SKILL.md                              Router. Picks the right reference for the task.
└── references/
    ├── content-strategy.md               Four pillars, three-layer arbitration model, chunkability, primary-source signaling, E-E-A-T, pre-publish checklist.
    ├── technical-implementation.md       FAST framework, Core Web Vitals, semantic HTML.
    ├── structured-data.md                JSON-LD for Article, FAQ, Organization, Product, HowTo, Person.
    ├── ai-crawlers-and-llmstxt.md        Crawler list, robots.txt, llms.txt formats.
    ├── platforms.md                      Per-engine optimization tactics.
    ├── agent-readiness.md                MCP, OAuth, x402, A2A, API Catalog, UCP.
    ├── measurement.md                    Benchmarks, GA4 regex for AI referral traffic, monitoring tools.
    ├── audit-checklist.md                Severity-graded audit with primary-source signaling check and final report template.
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
- Google, [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)
- Google, [agent-friendly website best practices](https://web.dev/articles/ai-agent-site-ux)
- Charles Floate, ["I Reverse Engineered LEAKED System Prompts For AI SEO"](https://x.com/Charles_SEO/status/2056323032973754825) (May 2026), for the three-layer arbitration framing and primary-source signaling analysis.

## License

MIT
