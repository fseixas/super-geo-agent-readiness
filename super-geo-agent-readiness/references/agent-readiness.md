# Agent Readiness

The newest optimization surface. AI search optimization makes content citable. Agent readiness makes a site usable by AI agents that fetch, parse, authenticate, and act.

Sources: Cloudflare's "Introducing the Agent Readiness score" (April 2026), agentready.org open standard, Anthropic Model Context Protocol spec, RFC 9727 (API Catalog), RFC 8414 / 9728 (OAuth metadata).

## The four agent-readiness dimensions

Cloudflare's framing, mapped to the agentready.org spec:

1. Discoverability: help agents find the product and the files they need.
2. Content for agents: serve content in a parseable, token-efficient form.
3. Capabilities: declare what the product can do and how to call it.
4. Identity, access, and commerce: prove who the agent is, scope access, accept payment.

Each dimension below covers what to implement, in roughly increasing order of effort.

## Discoverability

### Sitemap (SHOULD)

Standard XML sitemap at `/sitemap.xml`. Covered in `technical-implementation.md`. Still the first thing many agents fetch.

### robots.txt with AI policy (MUST per agentready.org)

A robots.txt that distinguishes AI crawlers from search crawlers. Detail in `ai-crawlers-and-llmstxt.md`.

### llms.txt (SHOULD)

Plain-Markdown index at `/llms.txt`. Detail in `ai-crawlers-and-llmstxt.md`.

### llms-full.txt (MAY)

Single-file dump of the full site content. Detail in `ai-crawlers-and-llmstxt.md`.

### HTTP Link header (MAY, but high-leverage)

Per RFC 8288. Agents can discover related resources without parsing HTML:

```
HTTP/1.1 200 OK
Content-Type: text/html
Link: </sitemap.xml>; rel="sitemap"
Link: </.well-known/api-catalog>; rel="api-catalog"
Link: </llms.txt>; rel="describedby"
Link: </.well-known/mcp/server-card.json>; rel="alternate"; type="application/json"
```

Implementation depends on the stack. Examples:

Nginx:
```nginx
add_header Link "</sitemap.xml>; rel=\"sitemap\"";
add_header Link "</llms.txt>; rel=\"describedby\"";
```

Next.js (in `next.config.js`):
```javascript
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
```

## Content for agents

### Markdown content negotiation (emerging, high-impact)

When an agent fetches a page and sends `Accept: text/markdown`, return a clean Markdown version instead of HTML. Cloudflare measured token reductions of up to 80% (typical: 60-75%) compared to the full HTML.

This matters because agents have context windows. An agent that can read three Markdown pages in one window might only fit one HTML page from the same site.

Implementation:

Nginx example:
```nginx
location ~* ^/(.+)$ {
  if ($http_accept ~* "text/markdown") {
    rewrite ^/(.+)$ /$1.md break;
  }
}
```

Cloudflare Rules approach:
1. URL Rewrite Rule: requests ending in `/index.md` rewrite to base path via regex_replace.
2. Request Header Transform Rule: set `Accept: text/markdown` based on the original path.

Node/Express:
```javascript
app.get('/:path(*)', async (req, res) => {
  const accept = req.headers.accept || '';
  if (accept.includes('text/markdown')) {
    const md = await loadMarkdownFor(req.params.path);
    res.type('text/markdown').send(md);
  } else {
    const html = await loadHtmlFor(req.params.path);
    res.type('text/html').send(html);
  }
});
```

### /index.md fallback (emerging)

Many agents do not send `Accept: text/markdown` by default. As of February 2026, only Claude Code, OpenCode, and Cursor request it. For everything else, expose Markdown at a URL suffix.

Convention: every page accessible at `/path/to/page` is also accessible at `/path/to/page/index.md` or `/path/to/page.md`.

Cloudflare's docs use both: header-based negotiation when the agent supports it, /index.md path-based fallback for everything else. Implement both.

### Open Knowledge Format (OKF) bundle (MAY, registration-layer bet)

OKF is a Google-published markdown standard (v0.1 draft, shipped inside the Knowledge Catalog product) for handing your content to AI agents as a clean, cross-linked corpus. A bundle is a directory of markdown files, each with YAML frontmatter, served at a stable path. It is the highest floor of the machine-readable layer: where `sitemap.xml` lists which URLs exist and `llms.txt` points an agent at the priority pages, an OKF bundle hands over the content itself as a graph the agent can walk.

Honest expectation setting. As of mid-2026 nothing crawls the web for OKF bundles. The spec is young and was built for data teams sharing tables and metrics, so pointing it at a website is a repurposing. A bundle will not move rankings or AI citations today. The case for shipping one is asymmetric cost: the format is plain markdown you can write in a text editor, and the cross-link graph you produce is a free structural audit (islands with nothing pointing in or out become obvious). Treat it the way schema was treated a decade ago: protocol-layer registration that you put down early and let compound, not advertising that pays off this quarter. It also reinforces the calibration argument in this skill, since Google's own AI guide calls "machine readable files... or Markdown" a myth while a different Google team published exactly that.

Minimal structure:

```
/okf/
├── index.md                 # Directory listing for progressive disclosure
├── log.md                   # Optional. Chronological change history, newest first
└── <concept>.md             # One markdown file per page or concept
```

Each concept file carries frontmatter with one required field and several recommended ones:

```
---
type: Article                                   # REQUIRED. Free string, self-describing. No central registry.
title: How to Connect the MCP Server            # Recommended. Display name.
description: One-line summary of the concept.    # Recommended. Used in index listings and previews.
resource: https://yoursite.com/blog/mcp-server/  # Recommended. Canonical URL of the underlying page.
tags: [mcp, integration]                         # Optional. Cross-cutting categories.
timestamp: 2026-06-14T00:00:00Z                  # Optional. ISO 8601 last-modified.
---

# How to Connect the MCP Server

The body of the page as clean markdown, with navigation and ads stripped out.
Cross-link to neighbours with bundle-relative links: see the [setup guide](/guides/setup.md).
```

Rules from the spec worth following:
- `type` is the only required field. Consumers must tolerate unknown type values, so pick descriptive self-explanatory strings rather than worrying about a fixed taxonomy.
- Use bundle-relative links beginning with `/` (stable when files move) over relative `./` links.
- Favor structural markdown (headings, lists, tables, fenced code) over prose in the body, since it aids both human reading and agent retrieval. This matches the chunkability guidance in `content-strategy.md`.
- `index.md` carries no frontmatter except an optional `okf_version: "0.1"` at the bundle root. It lists each concept with its description.
- Reserved filenames `index.md` and `log.md` cannot be used for concept documents.

Serving: drop the bundle at `yoursite.com/okf/` starting with `/okf/index.md`, and add a line in `llms.txt` pointing at it. On a static host or Cloudflare this is a drag and drop. Closed platforms that cannot serve files at a custom path are the one real wall.

A copy-paste starter bundle is in `templates.md`.

### Hidden agent directives

Inside every HTML page, include a comment that tells agents how to find the Markdown:

```html
<body>
<!--
STOP! If you are an AI agent or LLM, read this before continuing.
This is the HTML version of an Example Inc. documentation page.
Always request the Markdown version instead. HTML wastes context.
Get this page as Markdown: append /index.md to the URL.
All Example Inc. docs in one file: https://example.com/llms-full.txt
Site directory: https://example.com/llms.txt
-->
```

Strip the comment from the Markdown version (would cause a recursion loop).

### JSON-LD structured data (SHOULD)

Schema.org JSON-LD covered in detail in `structured-data.md`. Critical for both AI search citation and agent entity resolution.

### Speakable content markup (MAY)

For voice agents. Schema.org's SpeakableSpecification. Covered in `structured-data.md`.

## Capabilities

This is where agent readiness goes beyond AI search optimization. You are no longer publishing for an engine to read. You are publishing tools an agent can call.

### Model Context Protocol (MCP) server (MUST if you expose tools)

MCP is the open JSON-RPC protocol agents use to access external tools and resources. If your product exposes any callable capability (search your knowledge base, create a task, fetch user data, run a workflow), expose it as an MCP server.

Minimal MCP server example (using TypeScript SDK):

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server({
  name: "example-docs",
  version: "1.0.0",
}, {
  capabilities: { tools: {} }
});

server.setRequestHandler("tools/list", async () => ({
  tools: [{
    name: "search_docs",
    description: "Search the Example Inc. documentation by keyword.",
    inputSchema: {
      type: "object",
      properties: { query: { type: "string" } },
      required: ["query"]
    }
  }]
}));

server.setRequestHandler("tools/call", async (request) => {
  if (request.params.name === "search_docs") {
    const results = await searchDocs(request.params.arguments.query);
    return { content: [{ type: "text", text: JSON.stringify(results) }] };
  }
});

await server.connect(new StdioServerTransport());
```

For a Streamable HTTP transport (preferred for web-exposed MCP servers), use the HTTP transport from the SDK and host the server at a public endpoint like `https://example.com/mcp`.

### MCP Server Card (SHOULD when MCP server is exposed)

A JSON descriptor that lets agents discover an MCP server without connecting. Place at `/.well-known/mcp/server-card.json`:

```json
{
  "$schema": "https://static.modelcontextprotocol.io/schemas/mcp-server-card/v1.json",
  "version": "1.0",
  "protocolVersion": "2025-06-18",
  "serverInfo": {
    "name": "example-docs",
    "title": "Example Inc. Documentation MCP Server",
    "version": "1.0.0"
  },
  "description": "Search and retrieve content from Example Inc. documentation and knowledge base.",
  "transport": {
    "type": "streamable-http",
    "endpoint": "https://example.com/mcp"
  },
  "authentication": {
    "required": false
  },
  "tools": [
    {
      "name": "search_docs",
      "title": "Search Documentation",
      "description": "Search Example Inc. documentation by keyword or natural-language question.",
      "inputSchema": {
        "type": "object",
        "properties": {
          "query": { "type": "string", "description": "Search query." }
        },
        "required": ["query"]
      }
    },
    {
      "name": "get_page",
      "title": "Get Page",
      "description": "Retrieve a specific documentation page by URL or slug.",
      "inputSchema": {
        "type": "object",
        "properties": {
          "url": { "type": "string", "description": "Page URL or slug." }
        },
        "required": ["url"]
      }
    }
  ]
}
```

### A2A Agent Card (MUST when exposing an agent-to-agent surface)

For products that expose themselves as agents (not just tools). Place at `/.well-known/agent-card.json`. Spec: A2A v1.0 at https://a2a-protocol.org.

```json
{
  "name": "Example Research Agent",
  "description": "An agent that researches topics by combining web search and Example Inc.'s knowledge base.",
  "version": "1.0.0",
  "serviceEndpoints": [
    {
      "type": "streamable-http",
      "url": "https://example.com/agent/a2a"
    }
  ],
  "skills": [
    {
      "name": "research_topic",
      "description": "Given a topic, return a structured research summary with citations."
    }
  ],
  "authentication": {
    "type": "oauth2",
    "authorizationServer": "https://example.com/.well-known/oauth-authorization-server"
  }
}
```

### OpenAPI (MUST when exposing an HTTP API)

Machine-readable API description. Place the spec at a stable URL and reference from the API Catalog.

```yaml
openapi: 3.1.0
info:
  title: Example Inc. API
  version: 1.0.0
  description: HTTP API for the Example Inc. platform.
servers:
  - url: https://api.example.com/v1
paths:
  /pages/{slug}:
    get:
      summary: Retrieve a documentation page
      parameters:
        - name: slug
          in: path
          required: true
          schema: { type: string }
      responses:
        '200':
          description: Page content
          content:
            text/markdown:
              schema: { type: string }
            application/json:
              schema:
                type: object
                properties:
                  title: { type: string }
                  content: { type: string }
                  url: { type: string }
```

### API Catalog (MAY, but easy win for multi-API products)

Per RFC 9727. A linkset at `/.well-known/api-catalog` that lists all your APIs. Agents read this to discover what is available.

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

### Agent Skills index (MAY)

For products that ship reusable agent skills (capabilities packaged as portable SKILL.md files). Place at `/.well-known/agent-skills/index.json`:

```json
{
  "skills": [
    {
      "name": "search-example-docs",
      "version": "1.0.0",
      "description": "Skill that lets an agent search the Example Inc. documentation.",
      "url": "https://example.com/.well-known/agent-skills/search-example-docs/SKILL.md"
    }
  ]
}
```

## Identity, access, commerce

### OAuth 2.0 (MUST for user-owned resources)

For sites that require sign-in to access content or actions, expose OAuth so agents can request scoped access on behalf of the user.

### OAuth Authorization Server Metadata (MUST when running an OAuth server)

Per RFC 8414. Place at `/.well-known/oauth-authorization-server` or `/.well-known/openid-configuration`:

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

### OAuth Protected Resource (SHOULD on resource servers)

Per RFC 9728. Place at `/.well-known/oauth-protected-resource`:

```json
{
  "resource": "https://api.example.com",
  "authorization_servers": ["https://example.com"],
  "scopes_supported": ["read", "write"],
  "bearer_methods_supported": ["header"]
}
```

### PKCE (MUST for public clients)

Proof Key for Code Exchange with S256. Required for agent OAuth flows where the client cannot hold a secret. Standard in OAuth 2.1.

### Web Bot Auth (SHOULD)

HTTP message signatures (RFC 9421) over agent requests. Lets servers verify a request came from a specific agent operator.

Agents publish public keys at `/.well-known/http-message-signatures-directory`:

```json
{
  "keys": [
    {
      "kid": "openai-2026-01",
      "kty": "OKP",
      "crv": "Ed25519",
      "x": "..."
    }
  ]
}
```

Agents sign HTTP requests; servers verify the signature against the published key. Used for rate-limiting, abuse detection, and identifying friendly bots.

### x402 (MAY for API monetization)

HTTP-native payment protocol. Revives the HTTP 402 Payment Required status. Standard for stablecoin settlement of API calls.

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
      "address": "0x...",
      "expiry": "2026-05-18T18:30:00Z"
    }
  ]
}
```

Agent pays, retries with payment proof, server returns the resource. Useful for per-call API pricing.

### Agentic Commerce Protocol (MAY)

ACP. OpenAPI-backed checkout sessions for agent-driven purchases. Maintained by OpenAI and Stripe, used by ChatGPT Instant Checkout. Spec: https://github.com/agentic-commerce-protocol.

### Universal Commerce Protocol (MAY)

UCP. Two-sided capability negotiation between merchants and agents. Co-developed by Shopify and Google. Place at `/.well-known/ucp`.

Google explicitly endorses UCP in its [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) as the emerging protocol for letting Search agents do more on a site. This is the rare case where Google publicly aligns with a specific agent-readiness standard. For e-commerce sites that care about Google-driven agent traffic, UCP is no longer purely speculative.

### Browser agent UX (Google-endorsed reference)

Google publishes [agent-friendly website best practices on web.dev](https://web.dev/articles/ai-agent-site-ux). The focus is browser agents that read DOM, screenshots, and the accessibility tree. Useful complement to the protocol-level agent-readiness work in this file. Key practices:

- Semantic HTML with correct ARIA roles. Agents that parse the accessibility tree rely on this.
- Stable, machine-readable form labels. Avoid placeholder-only labels.
- Avoid CAPTCHAs on non-sensitive paths. Browser agents fail on them.
- Clear visual hierarchy. Helps screenshot-based agents identify primary actions.
- Predictable navigation patterns. Multi-step flows should not depend on hidden state.

Browser agents and protocol-level agents (MCP, OpenAPI) are complementary, not alternatives. The browser-agent path matters today; the protocol path becomes more important as agents mature.

## Cloudflare's "leading by example" patterns for large doc sites

These optimizations together produced 31% fewer tokens consumed and 66% faster answers when an agent queried Cloudflare's docs vs. competitor docs:

1. Per-directory `llms.txt`. Split a large `llms.txt` into one per top-level section. Root `llms.txt` points to the sub-files.
2. `/index.md` URL fallback for every page.
3. Strip directory listing pages from `llms.txt`. They are noise.
4. Rich frontmatter (title, description) on every page, used to populate `llms.txt` entries automatically.
5. Hidden agent directives in HTML, telling agents how to fetch Markdown.
6. Redirect AI training crawlers from deprecated pages to canonical pages. Prevents LLMs from being trained on outdated information.
7. Dedicated "LLM Resources" sidebar entry on every product directory, exposing `llms.txt`, `llms-full.txt`, and Agent Skills.

## Verify with isitagentready.com

The official Cloudflare scanner. Free, public, returns a score across all four dimensions plus actionable fixes for each failing check.

```
https://isitagentready.com/yourdomain.com
```

For each failing check, it returns a prompt you can paste into a coding agent (Claude Code, Cursor, etc.) to implement the fix.

Alternative scanners:
- ora.run Deep Scan (the official agentready.org scanner)
- Cloudflare URL Scanner with `agentReadiness: true` option

## Priority order for implementation

Most sites have everything in the discoverability and content sections within a single sprint. Capabilities and identity require more engineering.

Tier 1 (every site, no excuses):
1. robots.txt with AI crawler rules.
2. Sitemap.
3. llms.txt at root.
4. Schema.org JSON-LD.
5. HTTPS with HSTS.

Tier 2 (every site that publishes content):
6. Markdown content negotiation.
7. /index.md fallback.
8. Hidden agent directives.
9. Per-directory llms.txt for sites over 200 pages.

Tier 3 (products that expose tools or APIs):
10. MCP Server Card and an MCP server.
11. OpenAPI spec, API Catalog.
12. OAuth metadata if user accounts exist.

Tier 4 (specialized):
13. Web Bot Auth for sites running their own agents.
14. x402 / ACP / UCP for agentic commerce.
15. A2A Agent Card for agent-to-agent products.
16. OKF bundle at `/okf/` for content sites making the early registration-layer bet.

Anything past Tier 1 plus Schema.org makes a site visibly more agent-ready than 95% of the public web (April 2026 baseline: under 4% adoption for emerging standards).
