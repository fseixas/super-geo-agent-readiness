# Platform-Specific Tactics

How the major AI search engines select and cite sources. Platform behavior changes monthly. Treat this as a starting point and verify with a fresh web_search before making strong claims.

Industry traffic share as of July 2025 data (Semrush):

| Platform | Monthly visits | Share of AI referral traffic |
|---|---|---|
| ChatGPT | 5.24 B | 85.79% |
| Gemini | 287.5 M | 4.70% |
| Perplexity | 173.6 M | 2.84% |
| Grok | 153.0 M | 2.50% |
| Claude | 136.6 M | 2.23% |
| Copilot | 97.8 M | 1.60% |
| Meta AI | 16.6 M | 0.27% |

ChatGPT dominates by an order of magnitude. Optimize for ChatGPT first if you have to choose one. The other engines come essentially for free once the fundamentals (Schema, llms.txt, robots.txt, content quality) are in place.

## ChatGPT (OpenAI)

### How it sources content

ChatGPT search uses Bing as its primary index, layered with OpenAI's own retrieval. Pages that rank well in Bing have a much higher chance of being cited.

Key trait: ChatGPT pulls aggressively from Bing's results, including pages ranked outside Google's top 10. About 90% of ChatGPT citations come from URLs that rank 21+ in Google. Pages that struggle in Google can still earn ChatGPT citations.

50% of ChatGPT citation links point to business or service websites, vs. encyclopedic sources. The opposite pattern from Perplexity.

### How to optimize

- Register the site in Bing Webmaster Tools. Submit sitemap. Verify indexing.
- Build domain authority signals Bing favors: backlinks from .edu, .gov, news sites, industry publications.
- Win comparison and listicle queries. ChatGPT heavily cites "best X for Y" articles.
- Allow `OAI-SearchBot`, `ChatGPT-User`, and `GPTBot` in robots.txt.
- Submit product feed to OpenAI when shopping integration opens (currently in "register interest" mode).
- For SaaS or e-commerce, optimize the comparison and pricing pages with structured data.

### Common patterns

- ChatGPT often cites a mix of brand-owned and third-party content. The third-party content is where most lift comes from. Build mentions on Reddit, industry blogs, comparison sites, podcasts.
- Brand mentions in non-branded queries are common (26-39% of non-branded queries include at least one brand mention). The sentiment of those mentions matters.

## Perplexity AI

### How it sources content

Strong alignment with Google's top 10. About 91% domain overlap and 82% URL overlap with Google's first page. If you rank in Google, you have a high probability of being cited in Perplexity.

Perplexity is more transparent about sources than other engines; the citation list is prominently displayed alongside the generated answer.

### How to optimize

- Rank well in Google. The single biggest lever.
- Strong schema markup increases citation likelihood (Perplexity is a heavy schema consumer).
- Allow `PerplexityBot` and `Perplexity-User` in robots.txt.
- Write content that directly answers questions. Perplexity often quotes verbatim from the source paragraph.
- Comprehensive content with clear sections. Perplexity tends to cite multiple sources per answer; being one of several is easier than being the only one.

### Common patterns

- Perplexity favors content with strong topical authority (lots of related pages on the same domain).
- Multi-source citations are typical. Aim to be in the top 5 sources for a query, not just the top 1.

## Google AI Overviews

### How it sources content

AI-generated summaries that appear inside Google Search above the traditional results. About 15% of all keyword searches now trigger an AI Overview.

86% domain overlap with Google's top 10. The Overview is generated from Google's index plus light retrieval; if you rank well in classic Google, AI Overviews follow naturally.

Most common cited domains in AI Overviews: Quora #1, Reddit #2, with YouTube and Facebook appearing in 68% or more of Overview-augmented results.

### How to optimize

- Strong classic SEO is the foundation. AI Overviews are tightly coupled to ranking.
- FAQ schema is high-leverage. AI Overviews often quote directly from FAQ structured data.
- Optimize for E-E-A-T. Google explicitly applies E-E-A-T to AI Overview eligibility.
- Allow `Google-Extended` in robots.txt.
- Submit product feeds via Google Merchant Center for product citations.
- Be present in third-party content that ranks well (Quora, Reddit answers, Wikipedia mentions).

### Common patterns

- AI Overviews summarize and then link. Optimizing for both the citation (your snippet quoted) and the click-through (your link in the citation list) matters.
- Pages that get cited in AI Overviews see a click-through rate drop on the original snippet but a citation visibility gain. Net effect depends on intent.

## Google AI Mode

### How it sources content

The dedicated AI search experience inside Google (distinct from AI Overviews). Uses a more independent retrieval pipeline than classic Search.

54% domain overlap and 35% URL overlap with Google's top 10. About 7 unique domains appear in the sidebar that do not rank in the top 10.

### How to optimize

- Schema and llms.txt matter more here than in classic Google.
- Strong third-party citation profile (Reddit, YouTube, industry forums) helps.
- The same fundamentals (E-E-A-T, comprehensive content, structured data) carry over.

Treat AI Mode as separate from AI Overviews when measuring.

## Claude (Anthropic)

### How it sources content

Claude has built-in web search. Uses real-time fetching against external sources. Citation transparency is high.

Patterns are similar to Perplexity. Pages that rank in Google or Bing are more likely to be selected. Schema and llms.txt help retrievals.

### How to optimize

- Allow `ClaudeBot`, `anthropic-ai`, `Claude-Web` in robots.txt.
- Markdown content negotiation (`Accept: text/markdown`). Claude Code already requests this; the consumer Claude product is expected to follow.
- llms.txt at root.
- High-quality, well-structured content (Claude is trained to prefer clarity and accuracy).

### Common patterns

- Claude is more cautious than other engines about citing low-authority sources. Strong authority signals matter.
- Anthropic does not yet provide a Webmaster Tools equivalent. Standard SEO and GEO fundamentals are the primary lever.

## Gemini (Google)

### How it sources content

Gemini integrates with Google Search. Behavior overlaps significantly with AI Overviews when web grounding is enabled.

### How to optimize

- Same as Google AI Overviews. Strong classic SEO, schema, FAQ markup.
- Allow `Google-Extended` in robots.txt.
- Schema.org markup is heavily weighted.

## Copilot (Microsoft)

### How it sources content

Bing-powered. Patterns mirror ChatGPT, since both lean on Bing's index.

### How to optimize

- Bing Webmaster Tools registration. Sitemap. Verification.
- Submit product feed to Microsoft Merchant Center for product results.
- Same content principles as ChatGPT.

## Grok (xAI)

### How it sources content

DeepSearch combines real-time web fetch with X (Twitter) content. Heavy use of social signals.

### How to optimize

- Active X presence with authoritative voice on your topics.
- Brand mentions in X conversations.
- Standard GEO fundamentals.

Limited public information on Grok's retrieval architecture. Standard schema and llms.txt help.

## Universal patterns (apply across all platforms)

These hold regardless of platform:

- Comprehensive content beats thin content. AI engines synthesize across sources; thin content gets skipped.
- Original research is the highest-leverage citation magnet across all engines.
- Brand mentions in third-party content (Reddit, Quora, news, comparison sites, podcasts) drive citations independently of your own pages.
- E-E-A-T signals (named author, credentials, dates, transparent sources) are universal trust mechanisms.
- Comparison content earns disproportionate citations.
- LLMs often cite different pages from the same trusted domain. High domain overlap, lower URL overlap. Optimize at the domain level, not the page level.
- Commercial and transactional queries trigger responses roughly twice as long as informational ones. More citation slots available.

## When to use which platform's strengths

If the user has limited bandwidth, allocate effort by:

- **B2C consumer content**: ChatGPT first (85% of traffic), then Gemini and AI Overviews.
- **B2B research and comparison**: Perplexity and ChatGPT. Reddit and industry comparison sites for the citation surface.
- **Technical / developer content**: Claude (Anthropic's audience), Perplexity, Stack Overflow as the citation magnet.
- **E-commerce / product**: Google AI Overviews and ChatGPT shopping (when it opens). Product feeds to Google and Microsoft.
- **News / current events**: All platforms cite fresh content. Speed and frequency of publishing matter most.

## What does not change platform behavior

Be skeptical of any optimization advice that promises a specific lift per platform. Platform retrieval algorithms are opaque and change frequently. Some recurring myths:

- "Brand mentions cost X to acquire on platform Y." Acquisition costs vary by industry, geography, content type, and authority.
- "Reddit posts always get cited in Google AI Overviews." Reddit appears often, but the page-level selection is unpredictable.
- "Long content always outperforms short content." Length is a proxy for comprehensiveness. A 500-word page that answers fully can beat a 3000-word page that pads.

Treat platform-specific guidance as directional. Measure citation rates per platform (see `measurement.md`) and iterate based on what your data shows.
