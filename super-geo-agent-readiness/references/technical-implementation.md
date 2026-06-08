# Technical Implementation

The technical prerequisites that determine whether AI crawlers can fetch, parse, and trust your site. Without these in place, content quality is irrelevant.

## FAST framework: the crawlability test

The FAST framework (Fetchable, Accessible, Structured, Trim) is the fastest way to assess AI-crawler readiness.

### F: Fetchable

Can an AI crawler retrieve and read the HTML without executing JavaScript?

Test: load the page in your browser with JavaScript disabled (DevTools > Settings > Disable JavaScript). What remains is approximately what AI crawlers see. Many AI crawlers (GPTBot, ClaudeBot, PerplexityBot) do not execute JavaScript.

Common failures:
- React, Vue, Angular SPA that renders content client-side. Disabled JS shows a blank page.
- Content loaded via `fetch()` or `useEffect()` after page load.
- "Coming soon" placeholders that never resolve without JS.

Fix:
- Server-side rendering (SSR): Next.js with `getServerSideProps`, Nuxt with `asyncData`, Remix loaders, SvelteKit `load` functions, Astro SSR.
- Static site generation (SSG): same frameworks with `getStaticProps`/`generate` modes for content that does not change frequently.
- Incremental static regeneration (ISR): Next.js `revalidate`, on-demand revalidation for sites that update often at scale.

For new projects, default to SSG when possible (best performance, lowest cost). Switch to SSR when content is user-specific or changes per-request. Use ISR for hybrid cases.

### A: Accessible

Is the key content understandable without scripts?

Checks:
- All `<img>` tags have descriptive `alt` text. Decorative images use `alt=""`.
- All `<video>` and `<audio>` have captions or transcripts in the HTML.
- Heading hierarchy uses semantic `<h1>` through `<h6>` tags, not styled divs.
- Form labels are properly associated with `<label for="...">`.
- ARIA attributes are used correctly where native semantics are insufficient.

Bad:
```html
<div class="title">What is GEO?</div>
<div class="content">GEO is the practice of...</div>
```

Good:
```html
<article>
  <h1>Complete Guide to GEO</h1>
  <header>
    <p>By <a href="/authors/jane-smith" itemprop="author">Jane Smith</a></p>
    <time datetime="2026-05-01">May 1, 2026</time>
  </header>
  <section>
    <h2>What is GEO?</h2>
    <p>Generative Engine Optimization is the practice of...</p>
  </section>
</article>
```

### S: Structured

Are you using Schema.org, semantic HTML5 tags, and clear hierarchies?

Required:
- Schema.org JSON-LD in the `<head>` (see `structured-data.md`).
- Semantic HTML5: `<article>`, `<section>`, `<nav>`, `<header>`, `<footer>`, `<aside>`, `<main>`.
- Proper heading order: one `<h1>` per page, `<h2>` for top-level sections, `<h3>` for subsections.
- Lists for parallel items, tables for tabular data, code blocks for code.

### T: Trim

Are you sending only what is needed, with no bloat?

Checks:
- Page weight under 1 MB total transferred (including images).
- HTML body under 100 KB compressed.
- JavaScript bundle under 200 KB compressed, lazy-loaded where possible.
- No unused tracking scripts. Each one adds latency for the crawler.
- Compressed images (WebP or AVIF for raster, optimized SVG for vector).

Tools: Lighthouse, WebPageTest, PageSpeed Insights.

## Quick FAST audit

Run on the top 20 pages of any site:

```bash
# Disable JS check: fetch raw HTML and look for content
curl -sL https://example.com/page | grep -i "main keyword from page"

# If grep returns nothing, the content is JS-rendered. Failing F.

# Render with a JS-disabled headless browser
# (Puppeteer with javaScriptEnabled: false)

# Check response size
curl -sL -w "%{size_download}\n" https://example.com/page -o /dev/null
```

If F fails on a page, fix that before doing any other GEO work. The schema, the llms.txt, the content quality: none of it reaches the AI engine if the crawler gets nothing back.

## Core Web Vitals

AI crawlers have implicit timeouts. Slow pages get partial fetches or skipped entirely. Target:

| Metric | Target | Notes |
|---|---|---|
| LCP (Largest Contentful Paint) | Under 2.5 seconds | Most direct impact on crawler experience |
| INP (Interaction to Next Paint) | Under 200ms | Replaced FID in 2024 |
| CLS (Cumulative Layout Shift) | Under 0.1 | Affects content stability |
| TTFB (Time to First Byte) | Under 800ms | Critical for crawler efficiency |
| FCP (First Contentful Paint) | Under 1.8 seconds | Indicates SSR is working |

Tools: Google PageSpeed Insights, web.dev/measure, Chrome DevTools Performance tab, real user monitoring (RUM).

Common wins:
- Move to a CDN (Cloudflare, Fastly, Vercel Edge).
- Use HTTP/2 or HTTP/3.
- Inline critical CSS, defer non-critical CSS.
- Lazy-load below-the-fold images with `loading="lazy"`.
- Use `<picture>` with WebP/AVIF sources.
- Preload critical fonts with `font-display: swap`.

## Semantic HTML reference

Use these tags. Avoid div-soup.

| Tag | Use for |
|---|---|
| `<article>` | A self-contained piece of content (blog post, news article, comment) |
| `<section>` | A thematic grouping inside an article or page |
| `<nav>` | Navigation menus |
| `<header>` | Introductory content for a page or article |
| `<footer>` | Footer for a page or article |
| `<aside>` | Tangentially related content (sidebars, related links) |
| `<main>` | The primary content of the page (exactly one per page) |
| `<figure>` and `<figcaption>` | Images, diagrams, code samples with captions |
| `<time datetime="2026-05-01">` | Dates and times |
| `<address>` | Contact information |

## URL structure

Clean URLs help AI engines categorize content. Stable URLs let citations persist over time.

Good:
```
https://example.com/guides/generative-engine-optimization
https://example.com/blog/geo-vs-seo-comparison
https://example.com/glossary/llms-txt
https://example.com/case-studies/healthcare-saas-2026
```

Bad:
```
https://example.com/p?id=12345
https://example.com/blog/2026/05/18/post-title-here
https://example.com/content.php?category=seo&type=guide&id=789
https://example.com/blog/this-is-a-very-long-title-that-was-auto-generated-from-the-h1-and-keeps-going
```

Rules:
- Use lowercase, hyphenated slugs.
- Avoid date paths unless the content is genuinely time-bound.
- Avoid query strings for canonical URLs.
- Keep slugs under 60 characters.
- Stay close to the H1 (but trim, do not duplicate exactly).
- Once published, do not change the URL. Use 301 redirects if you must.

## Canonical URLs

Every page must declare its canonical URL via `<link rel="canonical">` and via the schema's `mainEntityOfPage`. Prevents duplicate content issues across www / non-www, http / https, tracking parameters, pagination.

```html
<link rel="canonical" href="https://example.com/guides/geo">
```

For paginated lists, point all pages to the first page as canonical (or use `rel="prev"` / `rel="next"` if pagination preserves unique content).

## HTTPS

Mandatory. AI crawlers strongly de-weight non-HTTPS sites. Use HSTS:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

Submit to the HSTS preload list at https://hstspreload.org once stable.

## Sitemap (XML)

Submit to Google Search Console, Bing Webmaster Tools, and reference in robots.txt.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2026-05-18</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

For sites over 50,000 URLs, use a sitemap index that points to multiple sitemap files. Generate from the CMS automatically. Update `lastmod` accurately, since AI crawlers use it to decide re-crawl priority.

## Image optimization for AI engines

AI engines increasingly cite images in multimodal answers (Google AI Overviews, ChatGPT vision search, Perplexity images tab).

For each image:
- Descriptive filename: `geo-citation-rates-by-platform.png`, not `IMG_4823.png`.
- Descriptive `alt` text: "Bar chart showing AI citation rates across ChatGPT, Perplexity, Claude, and Gemini, with ChatGPT leading at 47%". Not "chart".
- Caption with `<figcaption>` when context matters.
- Optimized format: WebP or AVIF. Provide a JPEG fallback if needed.
- Reasonable dimensions. Do not serve a 4000-pixel-wide image to a 800-pixel slot.
- Add `ImageObject` schema for hero images on important pages.

## Internal linking

AI engines crawl link graphs to discover content and infer topical authority.

Rules:
- Link from cornerstone content (your pillar pages) to supporting content.
- Use descriptive anchor text. Not "click here". Not "this article". The exact phrase the destination is about.
- Maintain a flat-ish architecture: most pages within 3 clicks from the homepage.
- Build topic clusters: a hub page surrounded by detail pages, all interlinked.
- Avoid orphan pages (no incoming internal links). Crawlers may miss them.

Anchor text examples:

Bad: "Read more about GEO [here]."

Good: "Read our complete guide to [generative engine optimization](https://example.com/guides/geo) for the full framework."

## Mobile-friendliness

Mobile-first indexing applies to AI crawlers too. Many simulate a mobile user agent or fetch the mobile rendering.

Verify:
- Single responsive site (preferred). Not m.example.com.
- Viewport meta tag present: `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- Tap targets at least 48 by 48 pixels.
- Font size at least 16px in body text.
- No horizontal scroll on standard mobile widths.

## Advanced: entity resolution

AI engines build entity graphs. Help them place your brand, product, and people on the right nodes.

- Consistent naming: use the same brand name everywhere. Avoid mixing "Acme", "Acme Inc.", "Acme Corp" interchangeably.
- Schema `sameAs`: link Organization and Person entities to Wikipedia, Crunchbase, LinkedIn, Twitter, GitHub.
- Use `mentions` in Article schema to flag entities discussed in the piece.
- Avoid name collisions. If your product name matches an existing entity (a common word, a different company's product), disambiguate with prefixes or alternate names in schema.

## Advanced: RAG adaptation

LLMs powered by Retrieval-Augmented Generation chunk and embed content. Optimize for retrieval:

- Modular content. Each section under each H2 should make sense alone.
- Front-load the answer in each section. The first paragraph after a heading is what often gets retrieved.
- Use semantic HTML so the chunker can identify boundaries.
- Avoid hidden text and JavaScript-rendered content (excluded from many embedders).
- Provide a Markdown version of the page (see `agent-readiness.md`).

## Verification checklist

Before declaring a site technically GEO-ready:

- [ ] All pages fetched with JS disabled return the expected content.
- [ ] Core Web Vitals pass on at least 75% of mobile sessions (per Search Console).
- [ ] HTTPS enabled site-wide with HSTS.
- [ ] Canonical URLs declared on every indexable page.
- [ ] XML sitemap submitted to Google Search Console.
- [ ] robots.txt allows AI search crawlers (see `ai-crawlers-and-llmstxt.md`).
- [ ] llms.txt published at site root (see `ai-crawlers-and-llmstxt.md`).
- [ ] Schema.org JSON-LD on every important page (see `structured-data.md`).
- [ ] Semantic HTML throughout (no div-soup).
- [ ] Internal linking forms clear topic clusters.
- [ ] Image alt text descriptive across the site.
- [ ] Site passes core checks at isitagentready.com (see `agent-readiness.md`).
