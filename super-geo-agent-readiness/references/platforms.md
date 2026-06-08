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

Keep the sizing in perspective. ChatGPT has reached roughly 12% of Google's search volume and has passed Bing, but Google still sends about 190x more traffic to websites than ChatGPT does (`ahrefs-2026-studies.md`). And 96.98% of Google clicks still land in the top 10 results. GEO is additive to SEO, not a replacement: AI answer engines are a fast-growing visibility layer where the unit of value is the citation, while the click economy still runs through page-one Google rankings. Measure both, and do not let AI-search hype pull all the budget off classic search.

## ChatGPT (OpenAI)

### How it sources content

ChatGPT search uses Bing as its primary index, layered with OpenAI's own retrieval. Pages that rank well in Bing have a much higher chance of being cited.

Key trait: ChatGPT pulls aggressively from Bing's results, including pages ranked outside Google's top 10. About 90% of ChatGPT citations come from URLs that rank 21+ in Google. Pages that struggle in Google can still earn ChatGPT citations.

A separate discovery layer exists. 28% of ChatGPT's most-cited pages have zero Google organic visibility (`ahrefs-2026-studies.md`). They get cited repeatedly while ranking nowhere in Google. The implication is concrete: register in Bing Webmaster Tools, not only Google Search Console, because the Bing-fed layer is its own surface.

What ChatGPT actually cites, by type (top 1,000 cited pages): Wikipedia 29.7%, homepages and landing pages 23.8%, educational pages 19.4%, app stores 6.6%, reviews 5.8%, news and media 5.2%, language and grammar sites 4.0%, dictionary and reference 2.2%, blogs and articles 1.9%, Q&A and community 0.9%, corporate pages 0.5%. Only about 32.3% of those citations sit in formats you can pitch your way into (educational, reviews, news, blogs). The other two-thirds are "dead" citations: Wikipedia, homepages, app store listings you influence slowly through authority, not through a content pitch. Concentrate content effort on the influenceable third, and build the off-site presence (Wikipedia entry, app store listing) separately and patiently.

The retrieval gate. ChatGPT retrieves dozens of URLs per query but cites only about 50% of them. A gate runs on the title, snippet, and URL before any page is opened (`ahrefs-2026-studies.md`). Titles that match query phrasing, snippets that answer rather than tease, and clean human-readable URLs all win at that gate. See `content-strategy.md`.

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

Intent is the strongest trigger. 99.9% of AI Overviews appear on informational-intent queries (`ahrefs-2026-studies.md`). Transactional, navigational, and local searches are almost entirely AIO-free, and shopping queries trigger one only 3.2% of the time. For AIO visibility, the battleground is informational, top-of-funnel content, not product or pricing pages.

86% domain overlap with Google's top 10. But citation is decoupling from ranking: only 38% of AI Overview citations now come from top-10 pages, down from 76% a year earlier. The Overview is generated from Google's index plus retrieval that increasingly reaches past the first page, so strong ranking still helps but is a weakening guarantee.

Volatility with stable meaning. An AI Overview's text has about a 70% chance of differing between consecutive observations and changes roughly every 2.15 days, yet semantic similarity across versions stays around 0.95. The sources and entities reshuffle constantly while the answer holds. Do not chase single-snapshot citations or panic at day-to-day churn; measure presence over weeks.

Click suppression is worsening. AI Overviews reduce clicks to the number-one organic result by 58%, up from 34.5% about ten months earlier. Expect declining organic click-through on informational queries even when rankings hold, and weight AIO visibility toward citation and brand presence rather than click volume.

Google's official position: AI Overviews are part of Google Search, not a separate engine. Optimization is SEO. See the [Google AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide). Google explicitly calls llms.txt, content chunking, AI-specific rewrites, and AI-specific structured data "myths" for their AI features.

Most common cited domains in AI Overviews: Quora #1, Reddit #2, with YouTube and Facebook appearing in 68% or more of Overview-augmented results.

### How to optimize

- Classic SEO at peak quality. AI Overviews are tightly coupled to ranking.
- Pass Google's "Search Essentials": indexed, snippet-eligible, no spam policy violations.
- Strong E-E-A-T. Google explicitly applies E-E-A-T to AI Overview eligibility.
- Helpful, non-commodity, people-first content. First-hand experience and original viewpoint outperform restated common knowledge.
- FAQ schema is high-leverage. AI Overviews often quote directly from FAQ structured data.
- Reduce duplicate content. Strong canonical URLs.
- Allow `Google-Extended` in robots.txt (controls inclusion in Gemini training and grounding, distinct from Googlebot).
- Verify the site in Google Search Console. Monitor AI Overview impressions and clicks via the standard Performance report.
- For products, submit feeds via Google Merchant Center.
- For local businesses, claim and maintain a Google Business Profile. Consider Business Agent for conversational brand presence in Search.
- Be present in third-party content that ranks well (Quora, Reddit answers, Wikipedia mentions, industry publications).

### What Google says you should ignore

Direct quote from their guide: you do not need llms.txt, content chunking, AI-specific rewrites, AI-specific structured data, or pursuit of inauthentic mentions. Implement those for other engines (ChatGPT, Perplexity, Claude) and expect zero direct benefit from Google.

### Common patterns

- AI Overviews summarize and then link. Optimizing for both the citation (your snippet quoted) and the click-through (your link in the citation list) matters.
- Pages that get cited in AI Overviews see a click-through rate drop on the original snippet but a citation visibility gain. Net effect depends on intent.

## Google AI Mode

### How it sources content

The dedicated AI search experience inside Google (distinct from AI Overviews). Uses a more independent retrieval pipeline than classic Search.

54% domain overlap and 35% URL overlap with Google's top 10. About 7 unique domains appear in the sidebar that do not rank in the top 10.

AI Mode and AI Overviews are two systems, not a short and long version of one. AI Mode responses run about 4x longer than AI Overviews, and the two reach the same conclusion 86% of the time while sharing only 13.7% of their citations (`ahrefs-2026-studies.md`). Being cited in one does not imply being cited in the other. Track them as separate surfaces, and do not assume an AIO win carries over to AI Mode. AI Mode also leans harder on branded authority signals (branded anchors, branded search volume) than ChatGPT does.

Google's official guidance: AI Mode is still Google Search. The same "myths" list applies. No special AI markup needed. The retrieval is more aggressive than AI Overviews but the optimization surface is the same.

### How to optimize

- Same fundamentals as AI Overviews: classic SEO at peak quality, E-E-A-T, helpful and original content, comprehensive coverage.
- Search Console verification and monitoring.
- Strong third-party citation profile (Reddit, YouTube, industry forums) helps because AI Mode reaches deeper into the long tail.
- Schema.org for rich results in classic Search. Per Google, not required for AI Mode itself, but the structured data tends to correlate with the same content quality signals AI Mode rewards.

Treat AI Mode as separate from AI Overviews when measuring. Track separately in Search Console reports as they are surfaced.

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

Gemini integrates with Google Search. Behavior overlaps significantly with AI Overviews when web grounding is enabled. Same official guidance as Google AI Overviews applies: classic SEO at peak quality, no special AI markup.

### How to optimize

- Same as Google AI Overviews. Classic SEO, E-E-A-T, helpful content.
- FAQ markup for rich results (incidentally helps).
- Allow `Google-Extended` in robots.txt. This is the relevant control for Gemini training and grounding (Googlebot remains separate).
- Search Console verification.

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
- "Best X" comparison lists are the single most-cited content format across assistants (43.8% of cited page types in one large study), and a high placement on credible third-party lists correlates with being recommended (`ahrefs-2026-studies.md`).
- YouTube presence is the strongest measured correlate of AI visibility, ahead of every conventional SEO metric. A video footprint (own channel plus mentions on others) plus off-site brand mentions move visibility more than on-page tactics.
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
