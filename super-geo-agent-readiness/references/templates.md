# Templates

Ready-to-paste configurations, code, and schemas. Adapt names and URLs to the user's domain. Do not invent fields.

## robots.txt (recommended)

```
# robots.txt for example.com

# Real-time AI search crawlers (allow for AI search visibility)
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

# AI training crawlers (allow or block based on your policy)
User-agent: CCBot
Allow: /

User-agent: Bytespider
Allow: /

User-agent: FacebookBot
Allow: /

User-agent: Meta-ExternalAgent
Allow: /

# Default: allow everything else
User-agent: *
Allow: /

# Block sensitive paths from all crawlers
Disallow: /admin/
Disallow: /private/
Disallow: /tmp/

Sitemap: https://example.com/sitemap.xml
```

## robots.txt (block AI training, allow AI search)

```
# Real-time AI search crawlers: allow
User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Perplexity-User
Allow: /

# AI training crawlers: block
User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: Google-Extended
Disallow: /

User-agent: Applebot-Extended
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: Bytespider
Disallow: /

User-agent: *
Allow: /

Sitemap: https://example.com/sitemap.xml
```

## llms.txt (small site)

```
# Brand Name

> 1 to 3 sentence summary of what the brand does and who it serves.

## About
- [Company overview](https://example.com/about): brief description of the page
- [How we work](https://example.com/how-we-work): brief description
- [Team](https://example.com/team): brief description

## Offerings
- [Product or Service 1](https://example.com/product-1): what it does and for whom
- [Product or Service 2](https://example.com/product-2): what it does and for whom

## Resources
- [Documentation](https://example.com/docs): brief description
- [Blog](https://example.com/blog): brief description
- [Case studies](https://example.com/case-studies): brief description

## Contact
- Email: hello@example.com
- Website: https://example.com
- LinkedIn: https://linkedin.com/company/example
```

## llms.txt (large site with per-directory pattern)

Root `/llms.txt`:

```
# Brand Name

> 1 to 3 sentence summary.

## Product Documentation
- [Product A](https://example.com/product-a/llms.txt): description
- [Product B](https://example.com/product-b/llms.txt): description

## Resources
- [API Reference](https://example.com/api/llms.txt): description
- [Tutorials](https://example.com/tutorials/llms.txt): description
- [Blog](https://example.com/blog/llms.txt): description
```

Per-directory `/product-a/llms.txt`:

```
# Brand Name — Product A

> What this product does in one sentence.

## Getting Started
- [Installation](https://example.com/product-a/install): how to install
- [Quickstart](https://example.com/product-a/quickstart): minimum viable example
- [Concepts](https://example.com/product-a/concepts): core ideas

## Reference
- [Configuration](https://example.com/product-a/config)
- [API](https://example.com/product-a/api)
```

## llms-full.txt (small site dump)

```
# Brand Name — Full Content

---
title: About
url: https://example.com/about
---

# About Brand Name

[Full content of /about pasted here, in Markdown.]

---
title: Pricing
url: https://example.com/pricing
---

# Pricing

[Full content of /pricing pasted here.]
```

## OKF bundle starter (serve at /okf/)

Google's Open Knowledge Format, v0.1. A bundle is a directory of markdown files served at `/okf/`. See `agent-readiness.md` for what it does and does not do (registration-layer bet, no crawler consumes bundles yet).

`/okf/index.md` (no frontmatter except the optional version declaration; lists every concept):

```
---
okf_version: "0.1"
---

# Articles

* [How to Connect the MCP Server](/articles/mcp-server.md) - The official servers, why they did not connect, and the fix.
* [AI Search Optimization Basics](/articles/ai-search-basics.md) - What changes when the reader is a model, not a person.

# Reference

* [Pricing](/reference/pricing.md) - Current plans and prices in plain text.
* [About](/reference/about.md) - Who we are and what we do.
```

`/okf/articles/mcp-server.md` (a concept file; `type` is the only required field):

```
---
type: Article
title: How to Connect the MCP Server
description: The official servers, why they did not connect, and the fix.
resource: https://example.com/blog/mcp-server/
tags: [mcp, integration]
timestamp: 2026-06-14T00:00:00Z
---

# How to Connect the MCP Server

The body of the page as clean markdown, navigation and ads stripped out.
Favor headings, lists, tables, and fenced code over prose.

Cross-link with bundle-relative links: see the [pricing reference](/reference/pricing.md).

# Citations

[1] [Official MCP documentation](https://modelcontextprotocol.io)
```

`/okf/log.md` (optional change history, newest first):

```
# Update Log

## 2026-06-14
* **Creation**: Added [How to Connect the MCP Server](/articles/mcp-server.md).

## 2026-06-10
* **Initialization**: Created bundle structure and root [index](/index.md).
```

Then add one line to `llms.txt` pointing at the bundle:

```
## Agent Resources

- [OKF bundle](https://example.com/okf/index.md): full site content as a cross-linked markdown corpus
```

## Article JSON-LD

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Article Title (match H1)",
  "description": "1 to 2 sentence meta description.",
  "author": {
    "@type": "Person",
    "name": "Author Name",
    "url": "https://example.com/authors/author-slug",
    "jobTitle": "Their Title",
    "sameAs": [
      "https://linkedin.com/in/author-handle",
      "https://twitter.com/author-handle"
    ]
  },
  "datePublished": "2026-05-18T09:00:00-03:00",
  "dateModified": "2026-05-18T09:00:00-03:00",
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
    "@id": "https://example.com/blog/article-slug"
  },
  "image": "https://example.com/blog/article-hero.jpg"
}
</script>
```

## FAQPage JSON-LD

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Question phrased the way users ask it?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Answer in under 100 words. Direct. Factual."
      }
    },
    {
      "@type": "Question",
      "name": "Second question?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Second answer."
      }
    }
  ]
}
</script>
```

## Organization JSON-LD (homepage)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Example Inc.",
  "alternateName": "Example",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "description": "What the company does, in 1 to 2 sentences.",
  "foundingDate": "2020-01-01",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Street 100",
    "addressLocality": "City",
    "addressRegion": "Region",
    "postalCode": "00000-000",
    "addressCountry": "Country code"
  },
  "sameAs": [
    "https://linkedin.com/company/example",
    "https://twitter.com/example",
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
</script>
```

## Product JSON-LD

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Product Name",
  "description": "What it does and for whom.",
  "brand": {
    "@type": "Brand",
    "name": "Brand Name"
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
    "bestRating": "5",
    "worstRating": "1",
    "reviewCount": "250"
  }
}
</script>
```

## HowTo JSON-LD

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to [verb] [object]",
  "description": "Brief description of the procedure.",
  "totalTime": "PT15M",
  "step": [
    {
      "@type": "HowToStep",
      "name": "Step 1 name",
      "text": "What to do in step 1."
    },
    {
      "@type": "HowToStep",
      "name": "Step 2 name",
      "text": "What to do in step 2."
    }
  ]
}
</script>
```

## Person JSON-LD (author or team page)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Full Name",
  "url": "https://example.com/team/person-slug",
  "image": "https://example.com/team/person-slug.jpg",
  "jobTitle": "Their Title",
  "worksFor": {
    "@type": "Organization",
    "name": "Example Inc.",
    "url": "https://example.com"
  },
  "sameAs": [
    "https://linkedin.com/in/person-handle",
    "https://twitter.com/person-handle",
    "https://medium.com/@person-handle"
  ],
  "knowsAbout": ["topic 1", "topic 2", "topic 3"]
}
</script>
```

## MCP Server Card

Place at `/.well-known/mcp/server-card.json`:

```json
{
  "$schema": "https://static.modelcontextprotocol.io/schemas/mcp-server-card/v1.json",
  "version": "1.0",
  "protocolVersion": "2025-06-18",
  "serverInfo": {
    "name": "example-server",
    "title": "Example Inc. MCP Server",
    "version": "1.0.0"
  },
  "description": "What this MCP server lets agents do.",
  "transport": {
    "type": "streamable-http",
    "endpoint": "https://example.com/mcp"
  },
  "authentication": {
    "required": false
  },
  "tools": [
    {
      "name": "tool_name",
      "title": "Human-readable tool title",
      "description": "What this tool does, when an agent should call it.",
      "inputSchema": {
        "type": "object",
        "properties": {
          "param_name": {
            "type": "string",
            "description": "What this parameter is for."
          }
        },
        "required": ["param_name"]
      }
    }
  ]
}
```

## A2A Agent Card

Place at `/.well-known/agent-card.json`:

```json
{
  "name": "Example Agent",
  "description": "What this agent does and who it serves.",
  "version": "1.0.0",
  "serviceEndpoints": [
    {
      "type": "streamable-http",
      "url": "https://example.com/agent/a2a"
    }
  ],
  "skills": [
    {
      "name": "skill_name",
      "description": "What this skill does."
    }
  ],
  "authentication": {
    "type": "oauth2",
    "authorizationServer": "https://example.com/.well-known/oauth-authorization-server"
  }
}
```

## API Catalog

Place at `/.well-known/api-catalog`:

```json
{
  "linkset": [
    {
      "anchor": "https://api.example.com",
      "service-desc": [
        {
          "href": "https://api.example.com/openapi.yaml",
          "type": "application/yaml"
        }
      ],
      "service-doc": [
        {
          "href": "https://docs.example.com/api",
          "type": "text/html"
        }
      ],
      "status": [
        {
          "href": "https://status.example.com"
        }
      ]
    }
  ]
}
```

## OAuth Authorization Server Metadata

Place at `/.well-known/oauth-authorization-server`:

```json
{
  "issuer": "https://example.com",
  "authorization_endpoint": "https://example.com/oauth/authorize",
  "token_endpoint": "https://example.com/oauth/token",
  "registration_endpoint": "https://example.com/oauth/register",
  "scopes_supported": ["read", "write", "agent"],
  "response_types_supported": ["code"],
  "grant_types_supported": ["authorization_code", "refresh_token"],
  "code_challenge_methods_supported": ["S256"],
  "token_endpoint_auth_methods_supported": ["none", "client_secret_basic"]
}
```

## OAuth Protected Resource

Place at `/.well-known/oauth-protected-resource`:

```json
{
  "resource": "https://api.example.com",
  "authorization_servers": ["https://example.com"],
  "scopes_supported": ["read", "write"],
  "bearer_methods_supported": ["header"]
}
```

## x402 Payment Required response

```
HTTP/1.1 402 Payment Required
X-402-Version: 1
Content-Type: application/json

{
  "scheme": "x402",
  "network": "base",
  "accepts": [
    {
      "amount": "0.001",
      "currency": "USDC",
      "address": "0xYourAddressHere",
      "expiry": "2026-05-19T18:30:00Z",
      "description": "Per-call API access"
    }
  ]
}
```

## HTTP Link headers

```
Link: </sitemap.xml>; rel="sitemap"
Link: </llms.txt>; rel="describedby"
Link: </.well-known/api-catalog>; rel="api-catalog"
Link: </.well-known/mcp/server-card.json>; rel="alternate"; type="application/json"
```

Nginx:

```nginx
add_header Link '</sitemap.xml>; rel="sitemap"';
add_header Link '</llms.txt>; rel="describedby"';
add_header Link '</.well-known/api-catalog>; rel="api-catalog"';
```

Next.js (`next.config.js`):

```javascript
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          { key: 'Link', value: '</sitemap.xml>; rel="sitemap"' },
          { key: 'Link', value: '</llms.txt>; rel="describedby"' }
        ]
      }
    ]
  }
}
```

## Hidden agent directive (in HTML)

Place near the top of `<body>`:

```html
<!--
STOP! If you are an AI agent or LLM, read this before continuing.
This is the HTML version of an Example Inc. page.
Always request the Markdown version instead — HTML wastes context.
Get this page as Markdown: append /index.md to the URL.
All Example Inc. content in one file: https://example.com/llms-full.txt
Site directory: https://example.com/llms.txt
-->
```

Strip this from the Markdown version of the page.

## Markdown content negotiation (Express middleware)

```javascript
// Serve Markdown when Accept: text/markdown is requested
app.use(async (req, res, next) => {
  const accept = req.headers.accept || '';
  if (accept.includes('text/markdown')) {
    const markdownPath = req.path.endsWith('/') 
      ? req.path + 'index.md' 
      : req.path + '.md';
    try {
      const md = await fs.readFile(`./content${markdownPath}`, 'utf-8');
      res.type('text/markdown').send(md);
      return;
    } catch (e) {
      // fall through to HTML
    }
  }
  next();
});
```

## GA4 AI Referral Traffic regex (re-paste)

In GA4 → Admin → Data display → Channel groups, set Source matches regex:

```
.*(chatgpt\.com|openai\.com|perplexity\.ai|claude\.ai|gemini\.google\.com|bard\.google\.com|you\.com|search\.brave\.com|copilot\.microsoft\.com|grok\.x\.ai|x\.com/grok|meta\.ai|deepseek\.com|kagi\.com|phind\.com|consensus\.app).*
```

## Sitemap.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2026-05-18</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/blog/post-slug</loc>
    <lastmod>2026-05-18</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
</urlset>
```

## HSTS header

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

Submit to https://hstspreload.org once stable.

## Adaptation notes

When pasting these templates:

1. Replace `example.com` with the user's domain.
2. Replace placeholder names (Example Inc., Brand Name, Author Name) with real values.
3. Update dates to current.
4. Validate JSON-LD at https://search.google.com/test/rich-results and https://validator.schema.org.
5. Validate llms.txt structure manually against https://llmstxt.org spec.
6. For OAuth, MCP, and A2A templates, verify the spec version against the latest at the source.
7. Test with `curl` after deploy. Real verification beats assumptions.
