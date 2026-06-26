# Super GEO Agent Readiness

A Claude Skill that covers Generative Engine Optimization (GEO) and agent readiness in one place. SEO targeted blue links. This skill targets the answers AI engines produce, and the agents that fetch and act on your content.

## Recent updates

### Network-traffic analysis of ChatGPT source selection (latest)

Integrated findings from Suganthan Mohanadasan's June 2026 analysis of ChatGPT's raw browser network traffic into the platform, content, technical, and measurement references.

1. **Four-tier source pipeline.** Every web result ChatGPT pulls is stamped internally with a `result_source` field carrying one of four values: `serp` (open web baseline), `labrador` (licensed allowlist of national publishers like Reuters, WSJ, Wikipedia, arXiv), `bright` (Bright Data scraper, dominant for shopping, finance, weather, local), and `oxylabs` (Oxylabs scraper, regional press). Most sites compete in the scraped tiers, so clean scrapability plus third-party coverage is the lever; the licensed tier is largely shut.
2. **Query triage.** Before searching, ChatGPT files the question into one of six `turn_use_case` buckets. The `text` bucket triggers no web search at all and answers from training, including some current, high-stakes queries. Confirm a target query actually triggers retrieval before investing in a page.
3. **Fetched, cited, and mentioned are three outcomes.** A page can be pulled into context, attached to a specific sentence, or named as a brand chip, and you can win or lose each independently. This independently confirms the retrieval-gate finding from the Ahrefs pass below: retrieval is not citation. Two unrelated methods now reach the same conclusion.
4. **The JavaScript pricing trap.** When pricing or specs sit behind JavaScript, the model cannot parse them and sources your numbers to a third-party aggregator instead. Captured in the model's own recorded reasoning. Facts that you want cited must sit in plain server-rendered HTML.
5. **One strong page per claim.** ChatGPT dedupes retrieval by domain, so volume content collapses. One comprehensive page that owns a claim beats many thin ones.
6. **DIY verification.** A free, reproducible method to read the pipeline behind any ChatGPT answer from your own browser's Network panel, added to the measurement reference.

These are one researcher's single-account observations. Structural findings (which fields exist, that `text` queries skip the web) are firm; frequency findings (which domain dominates, percentage splits) are directional and vertical-skewed. Verify before quoting.

### 14 Ahrefs studies (2025 to 2026)

Integrated 14 Ahrefs studies published between October 2025 and May 2026 (roughly 1 billion data points across ChatGPT, Google AI Overviews, and AI Mode) into an evidence base at `references/ahrefs-2026-studies.md`, with the findings threaded through the content, platform, structured-data, measurement, and audit references.

Three findings reorder the playbook:

1. **Schema markup does not move AI citations.** A controlled difference-in-differences study of 1,885 pages found no meaningful citation lift on any platform (AI Overviews -4.6%, AI Mode +2.4%, ChatGPT +2.2%, with the last two indistinguishable from zero). The naive 3x correlation between schema and citation turned out to be selection, not cause. Schema is now framed as hygiene for rich results and entity clarity, not as an AI-citation lever, and the audit was recalibrated to match (schema severity dropped from High to Medium).
2. **Off-site signals dominate.** Across 75,000 brands, YouTube mentions were the strongest correlate of AI visibility (~0.737), ahead of every conventional SEO metric, with off-site brand mentions close behind (0.66 to 0.71). Site page count barely correlated (~0.194). The skill now treats video and earned mentions as the highest-leverage work, with a new off-site presence check added to the audit.
3. **"Best X" lists are the most-cited format.** Comparison lists made up 43.8% of cited page types across assistants, and a high placement on credible third-party lists correlated with being recommended. The format wins when executed as a primary source, not as thin affiliate filler.

Other updates in that pass: a retrieval-gate section (ChatGPT cites only about half the URLs it retrieves, deciding from the title, snippet, and URL before it opens a page), refreshed Google AI Overview behavior (citations now 38% from the top 10, down from 76% a year earlier; 99.9% on informational queries; clicks to the top result down 58%), the finding that AI Overviews and AI Mode agree 86% of the time but share only 13.7% of their citations (so track them separately), and current market sizing (ChatGPT at ~12% of Google's search volume, while Google still sends ~190x more website traffic and 96.98% of clicks stay in the top 10).

These are single-vendor studies and mostly correlational. Verify against a second source before quoting any figure in client-facing work.

## What it covers

Four optimization surfaces, one routing layer:

- **Content.** Authority, quotability, comprehensiveness, structure, primary-source signaling, the retrieval gate, and the fetched/cited/mentioned distinction. The patterns that get a page cited by ChatGPT, Perplexity, Claude, and Google AI Overviews, plus the off-site presence (video, earned mentions, third-party lists) that the data shows matters most.
- **Technical site.** FAST framework, Schema.org JSON-LD, robots.txt for AI crawlers, `llms.txt` and `llms-full.txt` at small-site, large-site, and per-directory scales, and the JavaScript pricing failure mode that hands your facts to a competitor.
- **Platform tactics.** Per-engine optimization for ChatGPT, Perplexity, Google AI Overviews, AI Mode, Claude, Gemini, Copilot, and Grok, with current traffic-share and citation-behavior numbers, including ChatGPT's four-tier source pipeline and query-triage buckets, so you know where to spend effort.
- **Agent readiness.** MCP Server Cards, A2A Agent Cards, OpenAPI, API Catalog (RFC 9727), OAuth metadata (RFC 8414/9728), Web Bot Auth, x402, ACP, UCP, Markdown content negotiation, and Google's Open Knowledge Format (OKF) bundle. Compiled from Cloudflare's agent-readiness work and the agentready.org open specification.

## Per-engine calibration: policy vs engineering

Google publishes its own [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) and classifies several common GEO tactics as "myths" for Google AI features. The skill treats Google's guide as a policy document, not as engineering reality. Concrete signals undercut the "myths" framing: Google's own Lighthouse tool checks for `llms.txt`, a separate Google team published the Open Knowledge Format (a markdown-with-frontmatter standard for handing site content to agents) while the guide calls machine-readable markdown files a myth, two senior Googlers (John Mueller and Addy Osmani) have publicly given opposite advice on markdown pages, Anthropic's published Claude system prompt contains explicit source-quality filters that target SEO-pattern content, and modern retrieval architecture operates on chunks regardless of what the policy document says.

One item on that myths list has since been confirmed by controlled evidence: schema added for AI. The difference-in-differences study above found no AI-citation lift from JSON-LD, so on schema the skill sides with Google and keeps markup for rich results and entity clarity. The `llms.txt` and chunking items remain contested. Calibrate item by item rather than treating the whole list as right or wrong.

Operational conclusion: for Google AI Overviews and AI Mode, classic SEO at peak quality is the foundation. For ChatGPT, Perplexity, Claude, and training corpora, the additional surfaces in this skill apply. See `references/content-strategy.md` for the three-layer arbitration model (latent knowledge / active retrieval / arbitration) that explains the mechanism.

## What's inside

```
super-geo-agent-readiness/
├── SKILL.md                              Router. Picks the right reference for the task.
└── references/
    ├── content-strategy.md               Four pillars, three-layer arbitration model, retrieval gate, fetched/cited/mentioned, domain dedup, chunkability, primary-source signaling, E-E-A-T, pre-publish checklist.
    ├── technical-implementation.md       FAST framework, Core Web Vitals, semantic HTML, JavaScript pricing failure mode.
    ├── structured-data.md                JSON-LD for Article, FAQ, Organization, Product, HowTo, Person, plus the controlled evidence on what schema does and does not do.
    ├── ai-crawlers-and-llmstxt.md        Crawler list, robots.txt, llms.txt formats.
    ├── platforms.md                      Per-engine optimization tactics and current citation behavior, including ChatGPT's four-tier source pipeline and query triage.
    ├── agent-readiness.md                MCP, OAuth, x402, A2A, API Catalog, UCP, OKF bundle.
    ├── measurement.md                    Benchmarks, GA4 regex for AI referral traffic, off-site leading indicators, DIY network-traffic verification, monitoring tools.
    ├── ahrefs-2026-studies.md            Consolidated evidence base: 14 Ahrefs studies (2025 to 2026) on schema, listicles, YouTube, ChatGPT retrieval, and AI Overview behavior.
    ├── audit-checklist.md                Severity-graded audit with primary-source signaling, off-site presence, and retrieval-gate checks, plus a final report template.
    └── templates.md                      Every config in one file, ready to paste, including an OKF bundle starter.
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
- Google, [Open Knowledge Format (OKF) specification](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/HEAD/okf/SPEC.md) (v0.1 draft), the markdown bundle standard inside Knowledge Catalog.
- Charles Floate, ["I Reverse Engineered LEAKED System Prompts For AI SEO"](https://x.com/Charles_SEO/status/2056323032973754825) (May 2026), for the three-layer arbitration framing and primary-source signaling analysis.
- Suganthan Mohanadasan, ["How ChatGPT Actually Picks Sources"](https://suganthan.com/blog/how-chatgpt-picks-sources/) (June 2026), for the four-tier source pipeline, query-triage buckets, fetched/cited/mentioned distinction, JavaScript pricing failure mode, and DIY network-traffic verification.
- Ahrefs, AI search optimization research (2025 to 2026), 14 studies summarized in `references/ahrefs-2026-studies.md` and published on the [Ahrefs AI Search blog](https://ahrefs.com/blog/category/ai-search/).

## License

MIT
