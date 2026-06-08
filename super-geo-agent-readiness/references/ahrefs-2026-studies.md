# Ahrefs 2026 Studies: Empirical Evidence Base

A consolidated record of 14 Ahrefs studies published between October 2025 and May 2026, covering roughly 1 billion data points across ChatGPT, Google AI Overviews, and Google AI Mode. Use this file when a claim in another reference needs grounding, when a writer asks "where is the evidence for that", or when calibrating effort against what the data actually supports.

These are large-sample observational studies from one vendor (Ahrefs Brand Radar). Most use correlation, not controlled causation, with the schema study being the exception. Treat the numbers as directional and current as of mid-2026. Re-verify with a fresh source before quoting any figure in client-facing work, since platform behavior shifts monthly.

## The one-line version of what changed

Three findings reorder the playbook. First, schema markup does not move AI citations, so stop selling it as a citation lever. Second, YouTube presence and off-site brand mentions correlate with AI visibility far more strongly than on-page or technical signals, so the highest-leverage work is off the site. Third, "best X" comparison lists are the single most-cited content format, so winning and appearing on those lists matters more than any markup decision.

## Content format and selection

### "Best X" lists dominate citations (26,283 source URLs, 750 prompts)

Across software, product, and agency prompts, recently updated "best X" blog lists were the single most prominent page type ChatGPT cited, at 43.8% of all page types. The same pattern held across the other major assistants, and was slightly more pronounced in Google AI Overviews. The 1,000 most-cited pages on each platform, across 150M+ prompts, all included comparison listicles.

Position inside the list matters. Brands placed in the top third of a list were recommended more often than those lower down, with the trend holding even after controlling for list length. Brands that ranked themselves first in their own lists were still cited.

Freshness matters. 79.1% of cited lists had been updated in 2025, and 26% within the prior two months. More cited lists had been updated after first publication (57.1%) than published once and left alone.

Quality of host varies widely. About 35% of cited best-lists sat on low-authority domains, many of which looked built purely for link or citation farming. This is enabled by ChatGPT leaning on Bing, where questionable sites rank more easily than in Google.

Operational implication: publish and maintain a genuinely useful comparison or "best" page in your category, refresh it on a cadence, and pursue inclusion (placed high) on credible third-party lists. Execute the format as a primary source, not as thin affiliate filler. See `content-strategy.md`.

### Content length does not decide citations (thousands of pages)

Short and long content both earn AI Overview citations. Length is not a selection factor on its own. This confirms the long-standing caveat that word count is a proxy for comprehensiveness, not a lever. Cover the topic fully at whatever length that takes.

### Content volume barely correlates with visibility (75K brands)

The number of pages on a site showed almost no relationship with AI visibility (Spearman ~0.194). Publishing more pages is not a path to citations. Depth and off-site authority are.

## ChatGPT retrieval mechanics

### ChatGPT cites about half of what it retrieves (1.4M prompts, GPT-5.2)

ChatGPT fetches dozens of candidate URLs per query but cites only ~50% of them (49.98% cited in the sample). A gatekeeping layer runs before any page content is read: each retrieved result returns a title, a snippet, a URL, and an ID, and ChatGPT uses that metadata to decide which pages are worth opening. Being retrieved and being cited are different events.

Operational implication: the title, the snippet, and a clean human-readable URL do real work at the gate, before content quality is even assessed. Write titles and meta descriptions that match likely query phrasing and state the answer plainly. See `content-strategy.md`.

### Two-thirds of ChatGPT's top citations are off-limits (top 1,000 cited pages)

Breakdown of the 1,000 most-cited ChatGPT pages by type: Wikipedia 29.7%, homepages and landing pages 23.8%, educational pages 19.4%, app stores 6.6%, reviews 5.8%, news and media 5.2%, language and grammar sites 4.0%, dictionary and reference 2.2%, blogs and articles 1.9%, Q&A and community 0.9%, corporate pages 0.5%.

Only 32.3% of those citations sit in formats a marketer can influence (educational, reviews, news, blogs). The rest are "dead" citations: Wikipedia, homepages, and app store listings you cannot easily pitch your way into. The leverage is concentrated in that influenceable third, plus the off-site presence (Wikipedia entry, app store listing) that you build slowly through authority rather than content.

### A separate discovery layer exists (top 1,000 cited pages)

28% (28.3% in the thread summary) of ChatGPT's most-cited pages have zero Google organic visibility. They get cited repeatedly while ranking nowhere in Google. ChatGPT's reliance on Bing plus its own retrieval creates a discovery layer that classic Google SEO does not address. Register and optimize in Bing Webmaster Tools, not only Google Search Console.

## Google AI Overviews and AI Mode

### AI Overviews and AI Mode are two systems, not one (730K response pairs)

AI Mode responses run about 4x longer than AI Overviews. The two reach the same conclusion 86% of the time (semantic similarity) but cite almost entirely different sources, with only 13.7% citation overlap. They are distinct retrieval pipelines converging on similar answers by different paths. Being cited in one does not imply being cited in the other.

Operational implication: track AI Overviews and AI Mode as separate surfaces. A win in one is not a win in both.

### AI Overviews churn constantly but barely change their mind (43K keywords)

The text of an AI Overview has about a 70% chance of differing between consecutive observations, changing roughly every 2.15 days on average. Yet semantic similarity across versions stays around 0.95. The words, sources, and named entities reshuffle constantly while the underlying answer holds steady.

Operational implication: do not panic at day-to-day citation volatility, and do not chase single-snapshot wins. Measure presence over weeks, not single observations. One appearance is not durable visibility.

### Citations are decoupling from rankings (863K SERPs)

Only 38% of AI Overview citations now come from pages ranking in Google's top 10, down from 76% a year earlier. AI Overviews increasingly pull from outside the first page. Classic ranking still helps, but it is a weakening guarantee of citation.

### AI Overviews appear almost only on informational queries (146M SERPs, 86 factors)

99.9% of AI Overviews appear on informational-intent queries. Transactional, navigational, and local searches are almost entirely free of them. Shopping queries trigger an AI Overview just 3.2% of the time.

Operational implication: for AI Overview visibility, prioritize informational, top-of-funnel content. Transactional and local pages are not the battleground for AIO, though they remain relevant for ChatGPT and classic Search.

### AI Overviews keep eating clicks (click-through study)

AI Overviews reduce clicks to the number-one organic result by 58%, up from 34.5% about ten months earlier. The suppression is worsening as AIO rolls out to more countries and languages. Plan for declining organic click-through on informational queries even when rankings hold.

## Brand signals and authority

### YouTube is the strongest visibility correlate (75K brands, Spearman)

YouTube mentions showed the strongest correlation with AI visibility of any factor studied (~0.737), beating every conventional SEO metric and even branded web mentions. YouTube mention impressions followed closely (~0.717). Branded web mentions correlated at 0.66 to 0.71.

ChatGPT correlated weakly with classic authority metrics: branded search volume 0.352, Domain Rating 0.266. AI Mode leaned harder on branded authority signals, with branded anchors at 0.628 and branded search volume at 0.466. All three assistants largely surfaced the same brands (output overlap 0.779).

Correlation is not causation, and the study says so explicitly. Improving these metrics will not mechanically lift visibility. But the ranking of factors is a strong steer on where to invest: video and earned brand mentions over backlink counts and page volume.

Operational implication: a YouTube presence (your own channel plus mentions on other channels) and a steady stream of off-site brand mentions are higher-leverage than most on-page tactics. For a B2B services firm, that means founder and team video, podcast appearances, and being named in third-party content.

## Schema markup

### Adding schema did not move AI citations (1,885 treated pages, 4,000 controls)

This is the one causal study in the set, using a matched difference-in-differences design. Pages that added JSON-LD schema between August 2025 and March 2026 saw no meaningful citation uplift on any platform: AI Overviews -4.6% (small but statistically significant, with both treated and control pages already declining), AI Mode +2.4%, and ChatGPT +2.2% (both indistinguishable from zero).

A naive look at 6 million URLs had shown cited pages were nearly 3x more likely to carry schema than non-cited pages. The controlled study shows that gap is selection, not cause: schema lives on better-maintained sites that already publish stronger content and earn more links. The schema rides the wave; it does not create it.

Operational implication: keep schema for what it actually does, which is rich results in classic Search and entity clarity. Stop presenting it as an AI-citation lever, stop prioritizing a schema rollout over content and off-site authority work, and do not promise citation lift from markup. See `structured-data.md`.

## Market sizing and the click economy

### ChatGPT is large in queries, small in referrals (search volume study)

ChatGPT has reached about 12% of Google's search volume and has passed Bing. But Google still sends roughly 190x more traffic to websites than ChatGPT does. AI search is a real and growing discovery surface, yet it is not yet a primary traffic source for most sites.

### Clicks still concentrate in the top 10 (click-distribution study)

96.98% of Google clicks happen in the top 10 results. For the traffic that still flows from classic search, first-page ranking remains decisive.

Operational implication: GEO is additive to SEO, not a replacement. The click economy still runs through page-one rankings, while AI answer engines are an emerging visibility layer where citation, not click, is the unit of value. Measure both.

### AI Overview prevalence varies by country (108M queries)

AI Overview presence in Google results differs substantially across countries. For non-US markets, including Brazil, verify current AIO penetration for the target locale before assuming US-level prevalence. Coverage and behavior in Portuguese-language results may lag or differ from English.

## Source list

1. AI Overviews vs AI Mode comparison: https://ahrefs.com/blog/ai-overviews-vs-ai-mode/
2. AI brand visibility correlations: https://ahrefs.com/blog/ai-brand-visibility-correlations/
3. Best-lists research: https://ahrefs.com/blog/best-lists-research/
4. AI Overview change frequency: https://ahrefs.com/blog/ai-overview-change/
5. AI Overview triggers: https://ahrefs.com/blog/ai-overview-triggers/
6. ChatGPT's most-cited pages: https://ahrefs.com/blog/chatgpts-most-cited-pages/
7. Schema and AI citations: https://ahrefs.com/blog/schema-ai-citations/
8. Why ChatGPT cites pages: https://ahrefs.com/blog/why-chatgpt-cites-pages/
9. AI Overview citations from top 10: https://ahrefs.com/blog/ai-overview-citations-top-10/
10. ChatGPT vs Google search volume: https://ahrefs.com/blog/chatgpt-has-12-percent-of-googles-search-volume/
11. AI Overviews reduce clicks update: https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/
12. Short vs long content in AI Overviews: https://ahrefs.com/blog/short-vs-long-content-in-ai-overviews/
13. AI Overviews international: https://ahrefs.com/blog/ai-overviews-international/
14. Almost all clicks in the top 10: https://ahrefs.com/blog/almost-all-clicks-happen-in-the-top-10-results/
</file_text>