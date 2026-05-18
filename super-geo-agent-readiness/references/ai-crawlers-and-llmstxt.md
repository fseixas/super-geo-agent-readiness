# AI Crawlers and llms.txt

How to make a site readable by AI search crawlers and answer engines. Covers robots.txt configuration, Content Signals, llms.txt, llms-full.txt, and per-directory llms.txt strategy for large sites.

## The two-layer crawler problem

Two distinct classes of AI agents fetch your site:

1. AI search crawlers: power real-time answers in ChatGPT search, Perplexity, Google AI Overviews. Blocking them blocks your content from appearing in AI search results today.
2. AI training crawlers: ingest content to train future models. Blocking them stops your content from being learned, but does not affect current AI search visibility.

Treat them as separate decisions. Most brands want to allow search-time crawlers (visibility today) and have a deliberate policy on training crawlers.

## robots.txt: allow the right crawlers

Place at `https://yourdomain.com/robots.txt`. Most sites have one. Most are written for Googlebot and ignore AI crawlers entirely.

### Standard recommended configuration

```
# Real-time AI search crawlers (affect AI search visibility today)
User-agent: GPTBot
Allow: /

User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: Claude-Web
Allow: /

User-agent: anthropic-ai
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Perplexity-User
Allow: /

User-agent: Google-Extended
Allow: /

User-agent: Applebot-Extended
Allow: /

User-agent: Amazonbot
Allow: /

User-agent: cohere-ai
Allow: /

# AI training crawlers (decide based on your AI training policy)
User-agent: CCBot
Allow: /

User-agent: Bytespider
Allow: /

User-agent: FacebookBot
Allow: /

User-agent: Meta-ExternalAgent
Allow: /

# Default rule for everything else
User-agent: *
Allow: /

Sitemap: https://yourdomain.com/sitemap.xml
```

### Crawler identity quick reference

| User-agent | Operator | Purpose |
|---|---|---|
| `GPTBot` | OpenAI | Training data collection |
| `OAI-SearchBot` | OpenAI | ChatGPT search index |
| `ChatGPT-User` | OpenAI | Real-time browsing inside a ChatGPT conversation |
| `ClaudeBot` / `Claude-Web` | Anthropic | Training and search |
| `anthropic-ai` | Anthropic | Legacy identifier |
| `PerplexityBot` | Perplexity | Index for Perplexity answers |
| `Perplexity-User` | Perplexity | Real-time fetch during a Perplexity query |
| `Google-Extended` | Google | Opt-out signal for Bard/Gemini training (does NOT affect Googlebot) |
| `Applebot-Extended` | Apple | Apple Intelligence training opt-out |
| `Amazonbot` | Amazon | Alexa and Amazon AI |
| `cohere-ai` | Cohere | Training data |
| `CCBot` | Common Crawl | Open dataset used by many AI labs |
| `Bytespider` | ByteDance | TikTok / Doubao training |

Verify identities at the operator's official documentation before adding rules. New crawlers appear frequently and old ones rebrand.

### Selective blocking

To allow search crawlers but block training crawlers:

```
# Search-time crawlers: allow
User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: PerplexityBot
Allow: /

# Training crawlers: block
User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: Bytespider
Disallow: /
```

To allow training but protect specific paths (sensitive content, paid resources, gated docs):

```
User-agent: GPTBot
Disallow: /pricing/
Disallow: /customers/
Disallow: /docs/private/
Allow: /
```

### Common mistakes

- Using a single `User-agent: *` block and assuming AI crawlers will respect it the same way Googlebot does. Many AI crawlers only read their named directive.
- Blocking GPTBot to "protect content" while leaving OAI-SearchBot allowed. Both visit your site; only OAI-SearchBot affects ChatGPT search visibility. Decide each one separately.
- Forgetting to include the Sitemap directive.
- Leaving stale Disallow rules that block paths that no longer need blocking.

## Content Signals (emerging standard)

Content Signals adds a directive to robots.txt that declares what AI may do with your content, independent of whether a specific crawler can access it.

```
User-agent: *
Content-Signal: ai-train=no, search=yes, ai-input=yes

User-agent: GPTBot
Content-Signal: ai-train=no, search=yes
```

Three signals:
- `ai-train`: allow or disallow ingestion for model training.
- `ai-input`: allow or disallow use as grounding context at inference time (RAG).
- `search`: allow or disallow inclusion in search index results.

As of 2026 this is supported by Cloudflare, gaining traction, and adopted by about 4% of top sites. Add it if you want fine-grained AI usage control. Adoption is voluntary on the crawler side.

## llms.txt: the AI-readable site map

`llms.txt` is an emerging standard (proposed at llmstxt.org in September 2024) that gives AI models a structured, machine-readable summary of your site. Place at `https://yourdomain.com/llms.txt`.

Sites with proper llms.txt report roughly 24% more accurate brand descriptions in AI responses (Semrush 2025).

### Minimum viable llms.txt

```
# Softo

> Softo is an AI-native software company headquartered in Rio de Janeiro that builds custom software, AI solutions, and automation for medium and large organizations. The two main offerings are Outcome Pods (full delivery) and Foundations (AI discovery: Sprint and Blueprint).

## About
- [Company overview](https://softo.com.br/about)
- [How we work](https://softo.com.br/how-we-work)
- [Case studies](https://softo.com.br/case-studies)

## Offerings
- [Outcome Pods](https://softo.com.br/outcome-pods): delivery model for AI solutions, software development, and automation
- [Foundations Sprint](https://softo.com.br/sprint): free invitation-only 5-day boot camp that produces functional software in production
- [Foundations Blueprint](https://softo.com.br/blueprint): paid AI strategy engagement

## Tools
- [Pulse Tech Assessment](https://pulse.sof.to)
- [Reframe](https://reframe.sof.to)
- [Legacy Sunset](https://legacysunset.com)
- [The Good RFP](https://thegoodrfp.com)

## Contact
- Email: hello@softo.com.br
- Website: https://softo.com.br
```

Structure:
- Line 1: `# Brand Name` (H1 with canonical company name).
- Line 2: `>` blockquote with 1 to 3 sentence summary.
- Subsequent H2 sections group related links. Each link has a descriptive label and brief context.

### llms.txt for content-heavy sites

For sites with substantial documentation or blog content, list the most important pages explicitly:

```
# Example Inc.

> Example Inc. helps brands increase visibility in AI search engines through GEO consulting and engineering-as-marketing tools.

## Documentation
- [Getting Started](https://example.com/docs/start.md): quickstart guide
- [API Reference](https://example.com/docs/api.md): full HTTP API documentation
- [Concepts](https://example.com/docs/concepts.md): how the platform works

## Guides
- [GEO Implementation Guide](https://example.com/guides/geo.md)
- [Schema.org Patterns](https://example.com/guides/schema.md)

## Blog
- [Latest Articles](https://example.com/blog)
- [GEO Industry Trends 2026](https://example.com/blog/geo-trends-2026.md)
```

When possible, link directly to Markdown versions of pages (see "Markdown content negotiation" in `agent-readiness.md`). Markdown is 60 to 80% more token-efficient than HTML.

### llms-full.txt: the single-file dump

For smaller sites (under 100 pages), publish `llms-full.txt`: a single Markdown file containing the full contents of every important page concatenated together. Agents can ingest the whole site in one request.

```
# Example Inc. — Full Content

---
title: About
url: https://example.com/about
---

# About Example Inc.

[full page content here]

---
title: Pricing
url: https://example.com/pricing
---

# Pricing

[full page content here]
```

For large sites, llms-full.txt is impractical (would exceed context windows). Use per-directory llms.txt instead.

### Per-directory llms.txt for large sites

Cloudflare's approach for sites with thousands of pages:

1. Publish a root `/llms.txt` that lists top-level sections, each pointing to its own per-section llms.txt.
2. Each section publishes its own llms.txt at, for example, `/docs/llms.txt`, `/blog/llms.txt`, `/api/llms.txt`.
3. Each section llms.txt fits comfortably in an agent's context window.
4. Agents read the root first, identify the relevant section, then fetch that section's llms.txt.

Example root llms.txt:

```
# Cloudflare Developer Docs

> Developer documentation for Cloudflare products including Workers, R2, KV, D1, Pages, and more.

## Product Documentation
- [Workers](https://developers.cloudflare.com/workers/llms.txt)
- [R2](https://developers.cloudflare.com/r2/llms.txt)
- [Pages](https://developers.cloudflare.com/pages/llms.txt)
- [D1](https://developers.cloudflare.com/d1/llms.txt)
- [KV](https://developers.cloudflare.com/kv/llms.txt)
```

This pattern is the difference between an agent that finds the right page in one request and an agent that runs a grep loop, makes seven requests, and still misses the answer.

### llms.txt best practices

- Include rich descriptions for each link. Agents read the description to decide whether to fetch the page.
- Keep entries token-efficient. One line per page with a short description outperforms long blurbs.
- Update llms.txt whenever you add or remove important pages. Generate it from CMS frontmatter where possible.
- Point to Markdown versions of pages where available (`.md` URLs).
- Validate at https://llmstxt.org/ tooling or with a Markdown linter.
- Reference llms.txt and llms-full.txt in robots.txt as the `Sitemap` or via a `Link` header.

### Discovering llms.txt programmatically

Agents commonly check these paths in order:
1. `/llms.txt`
2. `/llms-full.txt`
3. `/.well-known/llms.txt` (less common)

Make sure `/llms.txt` returns Content-Type `text/plain` or `text/markdown`. Some hosts incorrectly serve it as `application/octet-stream`, which agents ignore.

## hidden agent directives in HTML

For sites that maintain HTML pages, add a hidden directive that tells agents how to fetch the Markdown version:

```html
<!--
STOP! If you are an AI agent or LLM, read this before continuing.
This is the HTML version of an Example Inc. page. Always request the
Markdown version instead — HTML wastes context.
- Markdown for this page: append /index.md to the URL, or
  send Accept: text/markdown to the same URL.
- All Example Inc. products in one file:
  https://example.com/llms-full.txt
- Site directory:
  https://example.com/llms.txt
-->
```

Place this comment near the top of the `<body>`. Do not include it in the Markdown version (would cause a recursion loop).

## Sitemap (still relevant)

Even with llms.txt, maintain a standard XML sitemap at `/sitemap.xml`. Many AI crawlers still consult sitemaps first.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2026-05-01</lastmod>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/guides/geo</loc>
    <lastmod>2026-04-15</lastmod>
    <priority>0.8</priority>
  </url>
</urlset>
```

Submit the sitemap URL in Google Search Console, Bing Webmaster Tools, and reference it from robots.txt.

## Verifying configuration

After deploying:

1. `curl -A "GPTBot" https://yourdomain.com/` and verify the page returns content (not a block page).
2. `curl https://yourdomain.com/robots.txt` and confirm the directives are intact.
3. `curl https://yourdomain.com/llms.txt` and confirm the file returns with Content-Type `text/plain` or `text/markdown`.
4. `curl -H "Accept: text/markdown" https://yourdomain.com/some-page` and confirm Markdown is returned if you implemented content negotiation.
5. Submit to https://isitagentready.com to verify all standards pass.
