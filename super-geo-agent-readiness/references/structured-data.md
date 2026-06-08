# Structured Data

Schema.org JSON-LD patterns that AI engines parse to understand content type, authorship, entities, and relationships. Implement as `<script type="application/ld+json">` in the page `<head>`.

## Why structured data matters for GEO

AI engines use structured data as a high-signal trust source. A page with valid Article schema, a named author, accurate dates, and linked Organization is parsed differently than the same page without markup. The schema acts as the ground truth the engine can rely on when human-readable text is ambiguous.

Google explicitly recommends JSON-LD over microdata or RDFa. AI engines have aligned on the same convention.

## Does adding schema increase AI citations? (the honest answer)

No, not on its own. This is the single most over-sold claim in GEO, so the skill states the evidence plainly.

A controlled study tracked 1,885 pages that added JSON-LD between August 2025 and March 2026, matched against 4,000 control pages, using difference-in-differences (`ahrefs-2026-studies.md`). Adding schema produced no meaningful citation lift on any platform: AI Overviews -4.6% (small but statistically significant, and both treated and control pages were already declining), AI Mode +2.4%, ChatGPT +2.2% (both indistinguishable from zero).

The confusion comes from a real correlation: a broad scan of 6 million URLs found cited pages were nearly 3x more likely to carry schema than non-cited pages. That gap is selection, not cause. Schema lives on better-maintained, more technically mature sites that already publish stronger content and earn more links. The schema rides those signals; it does not generate them.

So why implement it at all? Three reasons that hold up:

- Rich results in classic Search. FAQ, HowTo, Product, Article, and Review markup still drive enhanced SERP features, and classic Search still sends roughly 190x more website traffic than ChatGPT.
- Entity clarity. Organization and Person schema with `sameAs` links help engines bind your brand and people to the right knowledge-graph node. This is about disambiguation, not a citation multiplier.
- Near-zero cost once. It is cheap to ship and harmless to keep.

What this means operationally: ship schema once, correctly, as part of basic hygiene. Do not stage a multi-week schema project expecting AI citations to climb, do not promise clients a citation lift from markup, and move the saved budget to content quality and off-site authority, which is where the evidence points. See `ahrefs-2026-studies.md` and `content-strategy.md`.

The patterns below remain the correct implementation when you do add markup.

## Article schema (use on every blog post, guide, article)

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Complete Guide to Generative Engine Optimization",
  "description": "How to optimize content for AI search engines including ChatGPT, Perplexity, and Google AI Overviews.",
  "author": {
    "@type": "Person",
    "name": "Jane Smith",
    "url": "https://example.com/authors/jane-smith",
    "jobTitle": "Director of Content Strategy",
    "sameAs": [
      "https://linkedin.com/in/janesmith",
      "https://twitter.com/janesmith",
      "https://github.com/janesmith"
    ]
  },
  "datePublished": "2026-01-15T09:00:00-03:00",
  "dateModified": "2026-05-01T14:30:00-03:00",
  "publisher": {
    "@type": "Organization",
    "name": "Example Inc.",
    "url": "https://example.com",
    "logo": {
      "@type": "ImageObject",
      "url": "https://example.com/logo.png",
      "width": 600,
      "height": 60
    }
  },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://example.com/guides/geo-optimization"
  },
  "image": "https://example.com/images/geo-guide-hero.jpg",
  "keywords": ["GEO", "generative engine optimization", "AI search", "LLM optimization"]
}
```

Required fields: `@context`, `@type`, `headline`, `author`, `datePublished`, `publisher`. AI engines treat missing `author` or stale `dateModified` as negative signals.

## FAQPage schema (use on FAQ sections and Q&A pages)

The format maps almost one-to-one to how AI engines answer questions. High-leverage. Add to product pages, pricing pages, support pages, and blog posts with a Q&A section.

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is Generative Engine Optimization?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Generative Engine Optimization (GEO) is the practice of structuring content so AI answer engines such as ChatGPT, Perplexity, Claude, and Google AI Overviews cite it. Unlike SEO, which targets ranked search results, GEO targets the generated answers themselves."
      }
    },
    {
      "@type": "Question",
      "name": "How is GEO different from SEO?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "SEO optimizes for ranked links in traditional search engines through keywords and backlinks. GEO optimizes for citations in AI-generated answers through authority, quotability, factual accuracy, and structured data. The two strategies overlap but the success metrics differ."
      }
    }
  ]
}
```

Best practices: keep answers under 100 words. Match the question phrasing real users type. Limit to 10 to 15 questions per page.

## Organization schema (use on homepage and About page)

Establishes the canonical entity record for your company.

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Example Inc.",
  "alternateName": "Example",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "description": "Example Inc. helps brands increase visibility in AI search engines through GEO and agent-readiness consulting.",
  "foundingDate": "2020-01-01",
  "founder": {
    "@type": "Person",
    "name": "Founder Name"
  },
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Rua Example, 100",
    "addressLocality": "Rio de Janeiro",
    "addressRegion": "RJ",
    "postalCode": "20000-000",
    "addressCountry": "BR"
  },
  "sameAs": [
    "https://twitter.com/example",
    "https://linkedin.com/company/example",
    "https://github.com/example",
    "https://en.wikipedia.org/wiki/Example_Inc",
    "https://www.crunchbase.com/organization/example"
  ],
  "contactPoint": {
    "@type": "ContactPoint",
    "contactType": "customer support",
    "email": "support@example.com",
    "availableLanguage": ["English", "Portuguese"]
  }
}
```

The `sameAs` array is critical for entity linking. Include every authoritative external profile.

## Product schema (use on product and SaaS pages)

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "GEO Analytics Platform",
  "description": "Track and optimize brand visibility across ChatGPT, Perplexity, Claude, Gemini, and Google AI Overviews.",
  "brand": {
    "@type": "Brand",
    "name": "Example Inc."
  },
  "image": "https://example.com/product/hero.png",
  "offers": {
    "@type": "Offer",
    "price": "99.00",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock",
    "url": "https://example.com/pricing"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "reviewCount": "250"
  }
}
```

For SaaS, also add `SoftwareApplication` schema as a second JSON-LD block. AI shopping queries lean heavily on Product + Offer + AggregateRating.

## HowTo schema (use on step-by-step tutorials)

```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to Configure robots.txt for AI Crawlers",
  "description": "Step-by-step guide to allowing GPTBot, ClaudeBot, and PerplexityBot.",
  "totalTime": "PT10M",
  "supply": [
    {
      "@type": "HowToSupply",
      "name": "Access to your site's root directory"
    }
  ],
  "step": [
    {
      "@type": "HowToStep",
      "name": "Locate robots.txt",
      "text": "Find or create /robots.txt at the root of your site."
    },
    {
      "@type": "HowToStep",
      "name": "Allow AI crawlers",
      "text": "Add User-agent entries for GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot, and Google-Extended with Allow: / directives."
    },
    {
      "@type": "HowToStep",
      "name": "Verify",
      "text": "Fetch /robots.txt and confirm the directives are present and properly formatted."
    }
  ]
}
```

Use HowTo for "how to" queries. The schema directly maps to AI step-by-step answers.

## Person schema (use on author pages and team pages)

```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Fabio Seixas",
  "url": "https://softo.com.br/team/fabio-seixas",
  "image": "https://softo.com.br/team/fabio-seixas.jpg",
  "jobTitle": "Founder and CEO",
  "worksFor": {
    "@type": "Organization",
    "name": "Softo",
    "url": "https://softo.com.br"
  },
  "sameAs": [
    "https://linkedin.com/in/fabioseixas",
    "https://medium.com/@fabioseixas",
    "https://twitter.com/fabioseixas"
  ],
  "alumniOf": "Pontifícia Universidade Católica do Rio de Janeiro",
  "knowsAbout": ["AI", "agentic systems", "software development", "GEO"]
}
```

Use Person on every author bio. Link from Article's `author` field by URL.

## SoftwareApplication schema (use on app and SaaS product pages)

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Pulse Tech Assessment",
  "operatingSystem": "Web",
  "applicationCategory": "BusinessApplication",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.7",
    "reviewCount": "120"
  },
  "url": "https://pulse.sof.to",
  "description": "Diagnostic tool that measures AI dev maturity of an engineering organization."
}
```

## SpeakableSpecification (for voice agents)

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "speakable": {
    "@type": "SpeakableSpecification",
    "cssSelector": [".summary", ".key-points"]
  }
}
```

Use when content has portions designed to be read aloud by voice assistants. Mark the speakable section with a CSS selector.

## Review and AggregateRating

Add to Product, Organization, or LocalBusiness when you have legitimate reviews. Do not invent ratings. AI engines and Google's spam team detect fake review markup.

```json
"aggregateRating": {
  "@type": "AggregateRating",
  "ratingValue": "4.8",
  "bestRating": "5",
  "worstRating": "1",
  "reviewCount": "250"
}
```

## Implementation rules

1. Use JSON-LD inside `<script type="application/ld+json">` in the `<head>`. Do not use microdata or RDFa for new implementations.
2. Every Article must have `author`, `datePublished`, `dateModified`, `publisher`. Missing fields hurt citation rates.
3. Keep `dateModified` accurate. When you edit a page, update both the visible date and the schema. Stale `dateModified` reduces freshness signals.
4. Use `sameAs` aggressively on Person and Organization. Each link is an entity disambiguation hint.
5. Nest schemas logically: Article references Author (Person), who works for Publisher (Organization).
6. For Q&A sections within an Article, embed FAQPage as a separate JSON-LD block on the same page.
7. Validate before deploy. Use Google Rich Results Test (https://search.google.com/test/rich-results) and Schema Markup Validator (https://validator.schema.org).
8. One canonical schema per entity type per page. Do not duplicate Article schema for the same article.

## When to add schema (priority order)

1. Article schema on every blog post, guide, news piece. Highest leverage.
2. Organization schema on homepage and About page. Establishes entity.
3. FAQPage schema on top 10 content pages with Q&A sections. Maps directly to AI answers.
4. Product schema on every product or pricing page (commerce or SaaS).
5. Person schema on every author bio page.
6. HowTo on step-by-step tutorials.
7. SoftwareApplication on app and SaaS product pages.
8. SpeakableSpecification when voice surfaces matter (rarely).

## Common mistakes

- Mismatched data: schema says `datePublished: 2024-01-15` while the page visibly says "Updated March 2026". Engines flag this and may discount the page.
- Author as a plain string instead of a Person object with URL and `sameAs`.
- Organization schema without `sameAs`. The single biggest miss for entity linking.
- FAQ schema with answers that do not appear in the visible page content. Both Google and AI engines penalize this.
- Hardcoding fake ratings or review counts. AI engines often cross-check against external review sources.
- Implementing schema on a page that fails the FAST framework. The schema cannot save a page AI crawlers cannot fetch.
