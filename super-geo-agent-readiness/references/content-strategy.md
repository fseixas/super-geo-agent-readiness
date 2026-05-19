# Content Strategy

Content creation principles for earning AI citations and brand mentions across ChatGPT, Perplexity, Google AI Overviews, Claude, Gemini, and Copilot.

## The four content pillars

Every piece of content optimized for generative engines should pass all four pillars. Treat them as a drafting framework, not a post-edit checklist.

### 1. Authority

AI engines disproportionately cite content from sources with established trust signals. Build authority into the content itself, not just the surrounding site.

Do:
- Cite primary sources with links (peer-reviewed research, government data, official documentation, named studies).
- Include original research, first-party data, or unique datasets. LLMs heavily favor evidence they cannot find elsewhere.
- Name the author with full credentials. Link the author profile to LinkedIn, Twitter/X, GitHub, scholarly profiles via `sameAs` in Person schema.
- Reference specific named experts, studies, and organizations.
- Add a "last reviewed" date and keep it current. Schema's `dateModified` matters for citation likelihood.

Don't:
- Make vague claims ("many businesses report", "studies show") without links.
- Use anonymous authors or generic byline ("By the Team").
- Recycle definitions everyone else publishes. AI engines prefer the source, not the summary.

Example:

Bad: "AI search traffic is growing fast and many websites are seeing big gains."

Good: "Backlinko's 2025 analysis shows LLM-originated website traffic grew 800% year over year between Q2 2024 and Q2 2025. Across the 80 client sites we instrumented at Softo, the average lift in AI-referred sessions was 340% over the same window."

### 2. Quotability

AI engines extract self-contained passages. Write paragraphs that work as standalone answers when pulled out of context.

Do:
- Lead each section with a one-to-three-sentence definition or core answer. Elaboration follows.
- Embed specific numbers, named facts, and concrete examples.
- Use "definition boxes" for key terms. A short paragraph that starts "X is..." and ends with a complete idea.
- Write so a chunk of 200 to 400 tokens taken from anywhere in the article would still make sense.

Don't:
- Bury the definition under throat-clearing ("In the world of digital marketing, there are many strategies...").
- Use coreference that depends on prior paragraphs ("As mentioned earlier...", "This approach...").
- Write paragraphs longer than 4 to 5 sentences. AI chunkers prefer shorter passages.

Example:

Bad: "When we think about what makes content actually work in this new world, it's worth stepping back and considering the role of AI search engines and how they've changed everything."

Good: "Generative Engine Optimization (GEO) is the practice of structuring content so AI answer engines cite it. Unlike SEO, which targets ranked links, GEO targets the answers themselves. ChatGPT, Perplexity, Claude, and Google AI Overviews each surface a small set of trusted sources per query, and the goal of GEO is to be one of them."

### 3. Comprehensiveness

AI models favor content that fully covers a topic, including follow-up questions and edge cases.

Do:
- Cover the topic to "no obvious next question". If the reader could plausibly ask "but what about X?", answer X.
- Anticipate beginner and advanced reader paths in the same page.
- Address common misconceptions explicitly. AI engines often need to disambiguate; named misconceptions help.
- Include a how-to or step-by-step section even on conceptual topics.
- Add a comparison table when the topic involves choosing between options.

Don't:
- Cap an article at an artificial word count if the topic needs more.
- Skip the boring "what is it" basics on advanced pages. AI engines may need that context for query matching.

### 4. Structure

Well-structured content parses cleanly. AI engines rely on HTML semantics and heading hierarchy to identify quotable units.

Do:
- Use a clear H1 then H2 then H3 hierarchy. Skip H4+ unless the page genuinely demands it.
- Headings should be descriptive ("How GEO citations differ from SEO rankings"), not creative ("The Citation Question").
- Use lists for parallel items, tables for comparisons, code blocks for code.
- Place the core answer or definition immediately after each subheading.
- Implement Schema.org markup (see `structured-data.md`).

Don't:
- Use div-soup styling instead of semantic tags.
- Write a wall of prose with no internal structure.
- Hide the answer mid-paragraph behind a colorful lede.

## The three-layer arbitration model (why the pillars work)

Every frontier model evaluates content through three overlapping systems. The four pillars above are practical tactics; this section explains the underlying mechanism. Use it when a writer asks why the pillars matter, or when diagnosing why a high-quality page is not earning citations.

1. **Latent knowledge.** What the model learned during training. Background world knowledge, semantic relationships, reasoning patterns. The model has a baseline belief about your topic before it sees your content. Content that corroborates the model's prior beliefs has a tailwind. Content that contradicts them needs disproportionately strong evidence.
2. **Active retrieval.** What the model fetches when answering. Web search results, knowledge graphs, citations. The grounding layer. Your content competes against other retrieved sources in the same query window, not in isolation.
3. **Arbitration.** The layer that decides which of the above to trust. Anthropic's published Claude system prompt includes explicit arbitration rules: present-day questions require search regardless of model confidence; original sources beat aggregators; topics "subject to a lot of search engine optimization like product recommendations" are treated with skepticism. Similar mechanics exist in other frontier models, though most are not publicly documented.

The four pillars map to these layers:

| Pillar | Primary layer served | Why it matters |
|---|---|---|
| Authority | Arbitration | The source-quality filter decides which retrieved results count. Primary-source markers (named author, credentials, citations to peer-reviewed work, first-party data) lift you above the aggregator tier in the filter. |
| Quotability | Active retrieval | Extractable units enter the retrieval pool. Sections that depend on surrounding context get dropped before the model ever sees them. |
| Comprehensiveness | Latent knowledge | Covering the topic to the depth the model expects corroborates its prior beliefs. Missing the obvious follow-up question signals shallow coverage. |
| Structure | Active retrieval | Semantic HTML and heading hierarchy determine which units the retrieval system can extract cleanly. |

The operational implication is non-obvious: a piece of content that is well-written and accurate can still fail at the arbitration layer if it pattern-matches to mid-tier SEO output. The next section addresses that filter directly.

## Content types most likely to earn AI citations

Some content types massively outperform others for AI citation rates. Source: Semrush AI citation research; corroborated by Backlinko, Animalz, and the Princeton GEO paper (arxiv 2311.09735).

| Content type | Why it earns citations | Best practices |
|---|---|---|
| Original research | LLMs need evidence to support claims, and original data is uniquely citable | Run your own study. Publish methodology. Include downloadable raw data |
| Case studies | LLMs use case studies to back recommendations | Name the client. Specific numbers. Before/after format. Quote real people |
| Thought leadership | LLMs want diverse perspectives, not consensus restatements | Take a specific position. Disagree publicly with received wisdom. Sign with a real name and credentials |
| News and current events | LLMs cannot rely on pretrained data for recency | Cover developments fast. Date the piece prominently. Link to primary sources |
| Brand content (about your own product) | LLMs trust you to describe your own offering accurately | Write a dedicated product page with clear feature lists, pricing if public, comparison tables, FAQ |
| In-depth guides | LLMs prefer comprehensive sources over thin ones | 2000+ words. Cover the topic exhaustively. Internal links between related sections |
| FAQ pages | The format maps directly to how LLMs answer questions | Use FAQPage schema. One question per H3. Keep answers under 100 words |
| Comparison content | "Best X for Y" queries are heavily LLM-mediated | Side-by-side tables. Fair treatment of alternatives. Disclose your bias if you sell one of the options |
| Glossaries and definitions | "What is X" queries surface dictionary-style content | One definition per page. 50-200 word entries. Cross-link related terms |
| HowTo guides | Step-by-step queries cite HowTo content directly | Use HowTo schema. Numbered steps. Include screenshots or diagrams |

## Content types to avoid or de-emphasize

These rarely earn citations and dilute the rest of the site:

- Thin definitional content (under 300 words) that just restates what is already on Wikipedia.
- Purely promotional content with no information value ("We are the best at X").
- Outdated content that has not been refreshed. AI engines down-weight stale `dateModified`.
- Unsourced opinion pieces.
- Listicles padded with filler ("10 reasons we love AI").
- AI-generated content with no human editing. The detection tools are improving; even when undetected, the output rarely passes the four pillars.

## Look like a primary source, not like SEO content

Anthropic's published Claude system prompt explicitly instructs the model to be skeptical of "topics that are subject to a lot of search engine optimization like product recommendations, or any other search results that might be highly ranked but inaccurate or misleading." This is a real filter applied at the arbitration layer, before the model evaluates your content on its merits. Content that pattern-matches to mid-tier SEO output gets discounted regardless of how accurate or well-researched it actually is.

The visible markers the filter pattern-matches against:

- Formulaic listicle headers: "10 Best X for Y", "Ultimate Guide to Z", "Top N Tools in 2026"
- Thin definitional content that paraphrases Wikipedia
- Affiliate-heavy roundup structures with "winner / runner-up / budget pick" sections
- "Ultimate guide" or "complete guide" framing without original substance
- Unsourced authority claims: "experts say", "studies show", "research has proven"
- Generic stock-photo headers and CTA blocks repeated across articles
- Author bylines like "By the Team" or no byline at all
- Conclusion paragraphs that summarize the article rather than adding insight

The fix is not to abandon those formats. Lists, comparisons, and roundups have legitimate uses. The fix is to make the content read as a primary source even when it uses those formats:

- Named author with linked credentials and a face photo on the author profile.
- Specific numbers from your own measurement, not generic industry estimates.
- Direct quotes from named experts you actually spoke to, not paraphrases of secondary sources.
- Datasets you publish, not summaries of other people's datasets.
- A methodology section explaining how you reached your conclusions.
- Opinions clearly marked and defended, not hedged into safety.
- Caveats and falsifiers included where the analysis has limits.

A useful test: would a journalist citing this page quote it as a primary source, or as an example of "what the consensus says"? If the second, the page is in aggregator territory and will be filtered at the arbitration layer.

## Chunkability rules

LLMs that extract verbatim passages (Perplexity, ChatGPT, Claude) read content in chunks. Optimize for chunk independence to maximize citation likelihood on those engines.

Google's position: chunking is NOT required for AI Overviews or AI Mode. Their systems understand full-page context. The advice below is for the rest of the AI ecosystem, plus it improves human readability for everyone.

- A paragraph or section taken out of context should still make sense.
- Avoid pronouns that reach back across headings ("this approach" referencing a definition three sections up).
- Repeat the key entity name in each section where it is relevant ("Generative Engine Optimization" rather than "it").
- Avoid forward references ("as we will see below"). The chunk that contains "below" may not be the chunk the model retrieves.

Net advice: write for chunk independence anyway. The cost is near zero, the benefit covers most non-Google engines, and the readability lift is real.

## Entity clarity

AI engines build entity graphs internally. Help them link your brand, product, and people to the right node.

- Use the same canonical name for your company everywhere (no variants like "Acme", "Acme Inc.", "Acme Corporation" interchangeably).
- Add Organization schema to the homepage with `sameAs` linking to Wikipedia, Crunchbase, LinkedIn Company, X/Twitter, GitHub Organization.
- For people (authors, founders, experts), add Person schema with `sameAs` to their LinkedIn, scholarly profiles, official sites.
- For products, use Product schema with consistent name, brand, and SKU across all surfaces.
- Mention competitors and related entities by name when contextually relevant. This helps the engine place your brand in the right cluster.

## E-E-A-T applied to GEO

Google's E-E-A-T framework (Experience, Expertise, Authoritativeness, Trustworthiness) is the closest formal articulation of what AI engines reward.

| Element | What it signals | How to show it in content |
|---|---|---|
| Experience | First-hand use of what you describe | "We deployed this in production for 18 months", screenshots from real work, photos taken on site, named clients |
| Expertise | Subject-matter qualifications | Author bio with credentials, "Reviewed by [credentialed person]" line, citations to your own research, professional affiliations |
| Authoritativeness | Recognition from peers in the field | Press coverage, speaker bios, citations of your work by others, Wikipedia entries, industry awards, named partnerships |
| Trustworthiness | Transparent, accurate, secure, accountable | HTTPS, clear contact info, dated content, corrections appended visibly, privacy policy, author photos, real names |

For YMYL topics (Your Money or Your Life: health, finance, legal, safety), E-E-A-T is enforced harder. Add `reviewedBy` to Article schema, link to the reviewer's profile, and include verifiable credentials.

## Pre-publish content checklist

Verify before publishing:

- [ ] The page provides at least one piece of information not already on the top three SERP results.
- [ ] Verifiable facts have links to primary sources.
- [ ] Headings follow H1 then H2 then H3 hierarchy with descriptive labels.
- [ ] The first paragraph under each H2 stands alone as an answer.
- [ ] At least one of: original data, case study, named expert quote.
- [ ] Author is named with a linked bio and credentials.
- [ ] `datePublished` and `dateModified` are accurate in both visible content and schema.
- [ ] All claims older than 12 months that depend on volatile data have been verified.
- [ ] Schema.org JSON-LD is present (Article minimum, FAQ if applicable).
- [ ] The page reads coherently when any 300-word chunk is extracted.
- [ ] No em dashes. No promotional language. No "rule of three" filler.
- [ ] Internal links to related pages on the same site, with descriptive anchor text.
