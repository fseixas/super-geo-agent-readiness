# Audit Checklist

Complete pre-publish, technical, and post-publish audit for any page or site targeting AI search visibility and agent readiness. Use as a workflow when the user asks for a GEO audit.

## How to run an audit

1. Confirm scope. Single page? Whole site? Specific section?
2. If the user provides a URL, web_fetch it. Examine the rendered HTML. Check response headers. Look for `/robots.txt`, `/llms.txt`, `/sitemap.xml`, `/.well-known/` paths.
3. Run the checklist sections in order: content, structured data, technical, AI crawlability, agent readiness, measurement readiness.
4. Produce a report with three sections: passes, failures with severity, prioritized fix list mapped to reference files.
5. Offer to generate the fixes (code, JSON-LD, robots.txt) using `templates.md`.

Severity levels:
- **Blocker**: AI crawlers cannot reach the content. Nothing else matters.
- **High**: Major signal missing that prevents citation. Fix this quarter.
- **Medium**: Optimization opportunity. Improves citation rate.
- **Low**: Polish. Worth doing but not urgent.

## Calibration: Google's "myths" list

Before flagging items as failures, remember Google explicitly says these are NOT required for AI Overviews or AI Mode. They still help non-Google engines, so keep them in audits scoped to "all AI engines" and adjust severity for audits scoped only to Google.

| Item | Google AI features | Other engines (ChatGPT, Perplexity, Claude) |
|---|---|---|
| llms.txt | Not used. Severity Low for Google-only audit | Used. Severity High |
| Content chunking | Not required | Rewarded by extraction-style answers. Severity Medium |
| AI-specific rewrites | Not needed | Helps verbatim quoting. Severity Low |
| Structured data targeted at AI | Not required for AI features (still useful for rich results) | Helps citation. Severity Medium |
| Inauthentic mentions / link schemes | Explicitly counterproductive. Spam policy violation. Severity High if detected | Same. Counterproductive. |

When scoping an audit, ask the user: "Is this for visibility in all AI engines (ChatGPT, Perplexity, Claude, Google), or specifically for Google AI Overviews and AI Mode?" The severity weighting changes materially based on the answer.

For Google-scoped audits, prioritize: Search Console verification, classic Search Essentials compliance, E-E-A-T signals, helpful-content quality, Core Web Vitals, JavaScript SEO, duplicate content reduction, and Merchant Center or Business Profile feeds where relevant.

## Section A: Content audit (per page)

### A.1 Authority

- [ ] Author is named and has a linked bio page.
- [ ] Author has credentials visible (job title, organization, expertise).
- [ ] At least one primary source is cited with a link.
- [ ] If quantitative claims appear, they are sourced and dated.
- [ ] The page contains at least one piece of original information not on competitor pages.

Failures: High.

### A.2 Quotability

- [ ] The H2 leads with a definition or core answer in 1 to 3 sentences.
- [ ] Each section under an H2 stands alone when extracted.
- [ ] No paragraph exceeds 5 sentences without a structural break.
- [ ] Key facts are stated as facts, not buried in conversational prose.
- [ ] Pronouns do not reach across heading boundaries.

Failures: High.

### A.3 Comprehensiveness

- [ ] All major aspects of the topic are covered.
- [ ] Follow-up questions a reader would naturally ask are addressed.
- [ ] At least one of: how-to section, comparison table, or step-by-step guide.
- [ ] Common misconceptions are explicitly addressed where relevant.
- [ ] Word count is appropriate for the topic (typically 800 to 3000+ for guides).

Failures: Medium.

### A.4 Structure

- [ ] One H1 per page, descriptive (not generic like "Article").
- [ ] H2 and H3 hierarchy is logical and labels are descriptive.
- [ ] Lists used for parallel items, tables for comparisons, code blocks for code.
- [ ] Semantic HTML (`<article>`, `<section>`, `<nav>`) instead of div-soup.
- [ ] First paragraph after each heading contains the core answer.

Failures: Medium.

### A.5 Style and tone

- [ ] No em dashes.
- [ ] No promotional fluff ("revolutionize", "unleash", "unlock the power of").
- [ ] No vague "experts say" attributions without links.
- [ ] No rule-of-three filler.
- [ ] No "in conclusion" wrap-up paragraphs.
- [ ] Language matches the audience (Portuguese for BR audience, English by default).

Failures: Low.

## Section B: Structured data audit (per page)

### B.1 Schema presence

- [ ] JSON-LD present in `<head>`.
- [ ] At least one of: Article, FAQPage, Organization, Product, HowTo, Person, SoftwareApplication, applied appropriately.
- [ ] Validates with no errors in Google Rich Results Test.
- [ ] Validates with no errors in schema.org Validator.

Failures: High.

### B.2 Article schema (for content pages)

- [ ] `headline` present and matches H1.
- [ ] `author` is a Person object with `name`, `url`, optional `sameAs`.
- [ ] `datePublished` is present and accurate.
- [ ] `dateModified` is present, accurate, and matches visible date if displayed.
- [ ] `publisher` is an Organization object with `name`, `url`, `logo`.
- [ ] `mainEntityOfPage` set correctly.
- [ ] `image` present and the URL resolves.

Failures: High.

### B.3 Organization schema (on homepage and About)

- [ ] Present on homepage.
- [ ] `name`, `url`, `logo`, `description` populated.
- [ ] `sameAs` array contains at least 3 authoritative external profiles (LinkedIn, Twitter, Wikipedia, Crunchbase, GitHub, official press contact).
- [ ] `contactPoint` present where applicable.

Failures: High.

### B.4 FAQ schema (where applicable)

- [ ] FAQ schema present on FAQ pages and content pages with Q&A sections.
- [ ] Each Question has a clear `name` (the question).
- [ ] Each Answer is under 100 words, factual, complete.
- [ ] All questions and answers in the schema also appear visibly on the page.

Failures: Medium.

### B.5 Product schema (commerce or SaaS)

- [ ] Present on every product or pricing page.
- [ ] `name`, `description`, `brand`, `image` populated.
- [ ] `offers` with `price`, `priceCurrency`, `availability`.
- [ ] `aggregateRating` only if you have legitimate reviews. Never fake.

Failures: High for commerce, Medium for SaaS.

## Section C: Technical audit (per site)

### C.1 FAST framework

- [ ] **F: Fetchable**. Loading the top 20 pages with JavaScript disabled returns full content.
- [ ] **A: Accessible**. All images have descriptive alt text. Headings follow H1 to H6 properly.
- [ ] **S: Structured**. Schema.org JSON-LD on every content page. Semantic HTML throughout.
- [ ] **T: Trim**. Page weight under 1 MB. HTML body under 100 KB compressed.

Failures: Blocker for F. High for A, S. Medium for T.

### C.2 Core Web Vitals

- [ ] LCP under 2.5 seconds on mobile.
- [ ] INP under 200ms.
- [ ] CLS under 0.1.
- [ ] TTFB under 800ms.
- [ ] At least 75% of sessions pass Core Web Vitals (per Search Console).

Failures: High.

### C.3 HTTPS

- [ ] HTTPS enforced site-wide.
- [ ] HSTS header set with `max-age` at least 31536000.
- [ ] Site is on HSTS preload list (https://hstspreload.org).
- [ ] No mixed content warnings.

Failures: Blocker for HTTPS absence. High for missing HSTS.

### C.4 Canonical URLs

- [ ] `<link rel="canonical">` on every indexable page.
- [ ] Canonical matches the actual primary URL.
- [ ] Schema `mainEntityOfPage` matches the canonical.

Failures: Medium.

### C.5 Sitemap

- [ ] XML sitemap at `/sitemap.xml` or sitemap index for large sites.
- [ ] Submitted to Google Search Console and Bing Webmaster Tools.
- [ ] Referenced in robots.txt.
- [ ] `lastmod` dates accurate and updated on edits.

Failures: Medium.

### C.6 URL structure

- [ ] Clean, descriptive, lowercase, hyphenated.
- [ ] No query parameters in canonical URLs.
- [ ] Stable (no recent unnecessary URL changes).

Failures: Low.

### C.7 Mobile-friendliness

- [ ] Responsive (not separate m. subdomain).
- [ ] Viewport meta tag present.
- [ ] Tap targets at least 48x48 pixels.
- [ ] Body text at least 16px.

Failures: Medium.

## Section D: AI crawlability audit (per site)

### D.1 robots.txt

- [ ] File exists at `/robots.txt`.
- [ ] Allows the major AI search crawlers: GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, anthropic-ai, PerplexityBot, Google-Extended, Applebot-Extended.
- [ ] AI training crawler policy (CCBot, Bytespider, etc.) reflects an intentional decision.
- [ ] Sitemap directive present.
- [ ] No accidental Disallow rules blocking important paths.

Failures: Blocker if AI crawlers are blocked accidentally.

### D.2 llms.txt

- [ ] File exists at `/llms.txt`.
- [ ] Returns Content-Type `text/plain` or `text/markdown`.
- [ ] Starts with `# Brand Name` and `> 1-3 sentence summary`.
- [ ] Lists the key pages with descriptive labels.
- [ ] Updated when significant pages are added or removed.

Failures: High.

### D.3 llms-full.txt or per-directory llms.txt

- [ ] For sites under 100 pages: `/llms-full.txt` present with full content.
- [ ] For sites over 200 pages: per-directory `llms.txt` for each major section.
- [ ] Root `llms.txt` points to section-level llms.txt files where applicable.

Failures: Medium.

### D.4 Content Signals (optional but emerging)

- [ ] If a deliberate AI training / search / inference policy exists, `Content-Signal` directives in robots.txt match.

Failures: Low (emerging standard).

## Section E: Agent readiness audit (per site)

### E.1 Tier 1 (every site)

- [ ] robots.txt with AI policy (covered in D.1).
- [ ] Sitemap (covered in C.5).
- [ ] llms.txt (covered in D.2).
- [ ] Schema.org JSON-LD (covered in B).
- [ ] HTTPS with HSTS (covered in C.3).

### E.2 Tier 2 (content publishers)

- [ ] Markdown content negotiation: `Accept: text/markdown` returns Markdown.
- [ ] `/index.md` fallback works on at least the top 20 pages.
- [ ] Hidden agent directive comment present in HTML.
- [ ] HTTP Link headers expose key resources (sitemap, llms.txt, api-catalog).

Failures: Medium each.

### E.3 Tier 3 (products that expose tools or APIs)

- [ ] MCP Server Card at `/.well-known/mcp/server-card.json`.
- [ ] MCP server reachable at the declared endpoint.
- [ ] OpenAPI spec at a stable URL.
- [ ] API Catalog at `/.well-known/api-catalog` if multiple APIs exist.

Failures: High for MCP if the product is a candidate for one. Medium for OpenAPI.

### E.4 Tier 4 (identity, access, commerce)

Apply only if relevant:

- [ ] OAuth Authorization Server Metadata at `/.well-known/oauth-authorization-server`.
- [ ] OAuth Protected Resource at `/.well-known/oauth-protected-resource`.
- [ ] PKCE supported.
- [ ] Web Bot Auth public keys at `/.well-known/http-message-signatures-directory` (if running own agents).
- [ ] x402, ACP, or UCP if the product accepts agentic payments.

Failures: Medium each. Only if relevant.

### E.5 Scanner verification

- [ ] Site passes the checks at https://isitagentready.com.
- [ ] Site passes the agentready.org Deep Scan at https://ora.run.
- [ ] Score documented as baseline for future audits.

Failures: Medium.

## Section F: Measurement readiness audit

### F.1 Analytics

- [ ] GA4 installed and active.
- [ ] AI Referral Traffic custom channel group configured (regex from `measurement.md`).
- [ ] Goals or conversions defined that capture business outcomes.

Failures: High.

### F.2 Search Console

- [ ] Google Search Console set up and verified.
- [ ] Bing Webmaster Tools set up and verified.
- [ ] Sitemaps submitted in both.

Failures: Medium.

### F.3 AI monitoring

- [ ] At least one AI search monitoring tool active (Profound, Otterly, Peec, etc.).
- [ ] Top 20 to 200 strategic queries are being tracked.
- [ ] Baseline citation rate, brand mention rate, and visibility score documented.

Failures: Medium. Optional for very small sites.

### F.4 Reporting cadence

- [ ] Weekly traffic and citation review.
- [ ] Monthly content performance review.
- [ ] Quarterly content refresh and audit re-run.

Failures: Low.

## Report template

When delivering the audit, use this structure:

```
# GEO Audit: [Site or Page Name]
Date: [date]
Scope: [URL or scope description]

## Executive summary
[2 to 4 sentences. Lead with the most impactful failure or the most actionable win.]

## Passes
[Bulleted list of what is working.]

## Failures

### Blockers
[Items that prevent AI crawler access.]

### High priority
[Items that significantly hurt citation likelihood.]

### Medium priority
[Optimization opportunities.]

### Low priority
[Polish.]

## Prioritized fix list
1. [Specific fix]. Reference file: [filename]. Estimated effort: [low/medium/high].
2. ...

## Quick wins (ship this week)
[3 to 5 items that can be shipped in days, not weeks.]

## Next steps
[Recommended ordering and ownership.]
```

After delivering the audit, offer: "Would you like me to generate the implementation for any of these (code, schema, configuration)?" Use `templates.md` to produce ready-to-paste artifacts.
