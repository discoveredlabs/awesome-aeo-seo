# Awesome AEO & SEO [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Answer Engine Optimization (AEO) and Search Engine Optimization resources for technical engineers and growth marketers - research, tooling, and implementation guides.

## Contents

- [How AI Search Works](#how-ai-search-works)
- [Research & Papers](#research--papers)
- [AEO & GEO Fundamentals](#aeo--geo-fundamentals)
- [Content Engineering](#content-engineering)
- [Technical SEO](#technical-seo)
- [Measurement & Evaluation](#measurement--evaluation)
- [Reddit & Off-Page Signals](#reddit--off-page-signals)
- [Agentic SEO](#agentic-seo)
- [Open Source Tools](#open-source-tools)
- [People to Follow](#people-to-follow)
- [Related Lists](#related-lists)
- [Contributing](#contributing)

---

## How AI Search Works

Protocol-level and reverse-engineered analysis of how major AI search engines retrieve, rank, and synthesize content. Understanding retrieval mechanics is a prerequisite for any optimization work.

- [How ChatGPT Uses Reciprocal Rank Fusion for AI Citations](https://discoveredlabs.com/blog/how-chatgpt-uses-reciprocal-rank-fusion-for-ai-citations) - Covers ChatGPT's hybrid BM25 + vector retrieval pipeline fused via Reciprocal Rank Fusion with the scoring formula `1/(rank+k)` at `k=60`, including extracted code-level parameters (`rrf_alpha: 1`, `rrf_input_threshold: 0`) and a worked numerical example showing consistent mid-list presence (score 0.0462) outperforms a #1 ranking that appears in only one retrieval method (0.0164) - the clearest existing explanation of why retrieval consistency across methods matters more than raw authority for citation selection.

- [How Google AI Overviews Works](https://discoveredlabs.com/blog/how-google-ai-overviews-works) - Reverse-engineered five-phase pipeline (495ms initial HTML, async `/folsrch` endpoint, 200ms server-side retrieval, 1,200ms LLM synthesis, HTML fragment injection) with `mlro`/`mrc` routing token analysis; passage-level retrieval is scored independently of organic rank - a page's position in blue-link results does not predict AI Overview citation probability.

- [How Google AI Mode Works](https://discoveredlabs.com/blog/how-google-ai-mode-works) - Documents 816 simultaneous active experiments, five distinct backend routing paths, and a 6.5-second average response, showing why content must survive multiple routing configurations - not a single retrieval pass - to be reliably cited across AI Mode sessions.

- [How Gemini Works: AI Agent Architecture Deep Dive](https://discoveredlabs.com/blog/how-gemini-works-ai-agent-architecture-deepdive) - Protocol-level analysis of Gemini's BatchExecute RPC system cataloging 227 RPC calls across 28 methods per session, 138 active feature flags, six query routing modes (Default, Agent, Deep Research, Deep Think, Canvas, Gempix), and 161 long-polling Signaler connections - documents how structured data and entity relationships directly influence citation decisions at the session level.

- [Google AI Overviews: Official Indexing and Control Documentation](https://developers.google.com/search/docs/appearance/ai-overviews) - Google's canonical reference establishing that no special schema.org markup exists beyond standard Search eligibility and that exclusion is controlled via `robots.txt`, `nosnippet`, `data-nosnippet`, and `max-snippet` - authoritative on the absence of a privileged technical channel for AI Overview inclusion.

- [How AI Search Grounding Actually Works: Google vs OpenAI vs Anthropic](https://dejan.ai/blog/grounding/) - Direct comparison of grounding behavior across three platforms on the same query: Google retrieves 7 pages and cites all 7 (1:1 ratio), OpenAI retrieves 39 but cites only 2 (20:1 filtering), Anthropic reads 14 and cites 9 with visibility into 5 rejected sources; each wraps snippets differently (redirect-wrapped readable URLs, 100-200 word sliding windows, encrypted snippets requiring a second pass); establishes that "AI mentioned us" is only meaningful within a specific platform's citation architecture.

- [How Search Grounding Biased an LLM Against YouTube](https://dejan.ai/blog/how-search-grounding-biased-an-llm-against-youtube/) - Documents a case where grounding pulled from affiliate-driven vendor blogs rather than primary documentation, causing Claude to rank paid webinar platforms above the free incumbent; the mechanism: "retrieval does not correct the model's prior - it confirms it"; directly implies that brand presence in affiliate-independent, primary documentation is a stronger long-term AEO asset than presence in review aggregators.

- [LLM Knowledge Cutoff Dates: Every Major Model Fact-Checked](https://metehan.ai/articles/llm-knowledge-cutoff-dates) - Verified cutoff dates across GPT-4o (Oct 2023), GPT-5 variants (Sep 2024–Dec 2025), Claude Fable 5 and Opus 4.8 (Jan 2026), Gemini 3.5 Flash (Jan 2025 - stale despite May 2026 release), Qwen, Kimi, and MiniMax; the authoritative quick-reference for understanding which topics fall inside vs. outside parametric knowledge, directly determining where live-web AEO has leverage vs. where training data coverage makes retrieval unlikely.

- [Perplexity API Documentation](https://docs.perplexity.ai) - Covers the Agent API (third-party model access with web search tools, JSON schema structured responses, domain filtering), Search API (real-time ranked results with `search_domain_filter` and `search_recency_filter` parameters), and Embeddings API - the practical integration reference for engineers querying Perplexity in RAG pipelines or testing citation behavior programmatically.

- [LiveBrowseComp: Are Search Agents Searching, or Just Verifying?](https://arxiv.org/abs/2605.28721) - AI search agents answer ~45% of questions without triggering a search (Fan et al., 2026); queries are internally generated from model hypotheses rather than user-supplied queries, meaning content must match the model's natural framing of a topic - optimizing for the language a model would use to query your subject area matters as much as keyword targeting.

- [Navigating Uncertainty: Optimizing API Dependency for Hallucination Reduction](https://arxiv.org/abs/2401.01780) - LLMs trigger external web retrieval for only 62% of queries (Erbacher et al., 2024); topics well-covered in training data bypass live retrieval entirely, confining live-web AEO leverage to niche, emerging, and fast-changing domains where parametric knowledge is insufficient.

- [Uncertainty-Aware Web-Conditioned Scientific Fact-Checking](https://arxiv.org/abs/2604.11036) - Retrieval fires only when internal confidence on a specific atomic claim is insufficient (Vinod and Erk, 2026); to be cited, content must address the exact gap in the LLM's parametric knowledge - granular, verifiable claims on under-trained topics maximize citation probability in production retrieval pipelines.

- [How ChatGPT Shopping Works and Where Ads Fit In](https://discoveredlabs.com/blog/how-chatgpt-shopping-works-and-where-ads-fit-in) - Traffic inspection of 1.1 million product tracking parameters reverse-engineers ChatGPT's shopping infrastructure: the "Sonic" client-side query classifier (0.1ms execution), `connector_openai_shopping` architecture, Google Shopping API integration with `srsltid=AfmBOo*` tracking markers, and product ranking factors; documents zero current advertising infrastructure with full technical insertion points mapped for future monetization.

- [How Google AI Overviews Ads Work Today and Where They're Heading](https://discoveredlabs.com/blog/how-google-ai-overviews-ads-work-today-and-where-theyre-heading) - Network traffic analysis revealing AI Overview ad architecture: AI content renders 7ms before ad tracking fires, a unified `adview_query_id` links impressions to specific queries, and separate slot systems (`SLOT_TYPE_ABOVE_FEED` vs `SLOT_TYPE_AD_BREAK_REQUEST`) currently exclude ads from AI-generated content while staging infrastructure for future monetization.

- [How Google AI Mode Ads Work Today](https://discoveredlabs.com/blog/how-google-ai-mode-ads-work-today-and-what-they-might-look-like-tomorrow) - Analysis of 547 network flows from AI Mode sessions: ad auctions run and complete in the background within 60 milliseconds despite ads not yet displaying; documents AsyncDataService timing patterns, `adview_query_id` attribution chains, and shopping ad code patterns confirming the full monetization infrastructure is built and awaiting activation.

- [How Gemini Ads Work Today](https://discoveredlabs.com/blog/how-gemini-ads-work-today-and-what-they-look-like-when-activated) - Eight-layer traffic inspection revealing five dormant advertising SDKs (qads, ada, adrc, qapid, adcgm3) calling Google's OneGoogle Ads RPC endpoint and returning null responses; documents Shopify and Google Merchant Center commerce integrations alongside 5,000+ feature flags providing granular ad rollout control.

---

## Research & Papers

Controlled experiments, large-scale audits, and empirical studies on AI citation behavior. Methodology notes are included where sample size or experimental design affects how far findings generalize.

### Citation Drivers & Content Signals

- [What Gets Cited: Competitive GEO in AI Answer Engines](https://arxiv.org/abs/2605.25517) - 252,000-trial controlled experiment across six LLMs evaluating 18 content factors in head-to-head source competition (Vishwakarma et al., 2026); topical relevance and list position dominate, explicit pricing and recent timestamps provide consistent marginal lift, and formatting changes alone have minimal impact - directly falsifying a common GEO assumption.

- [Discovered Labs: What Drives AI Citations](https://discoveredlabs.com/research/what-drives-ai-citations) - ~2 million prompt-engine-citation data points across 10,000 cited URLs with 60+ attributes per page and nine robustness validation checks; prompt-content alignment is the dominant signal (β=+0.37, ~3× larger than competing signals), domain authority SHAP 0.38 vs. 0.06 for the strongest page-level feature, FAQ sections and TLDR blocks are positive, listicle reviews are negative, Core Web Vitals and schema show no independent citation signal, and engine recency preferences diverge: Claude cites median 5.1-month content, ChatGPT cites median 8.0-month content.

- [From Citation Selection to Citation Absorption](https://arxiv.org/abs/2604.25707) - Distinguishes being cited from having cited content actually influence the generated answer (Zhang et al., 2026); across 21,000+ citations on ChatGPT, Gemini, and Perplexity, ChatGPT cites fewer sources but absorbs more per citation, while high-absorption pages share longer content, strong structural hierarchy, and rich evidence including definitions, numerical facts, and comparisons.

- [From Experience to Skill: MAGEO](https://arxiv.org/abs/2604.19516) - Multi-agent framework that learns and reuses engine-specific editing strategies across documents rather than optimizing each instance from scratch (Wu et al., ACL 2026); introduces the DSV-CF metric jointly measuring semantic visibility and citation fidelity, revealing that pure visibility gains can hurt attribution accuracy - a finding absent from single-metric GEO benchmarks.

- [Authority Signals in AI Cited Health Sources](https://arxiv.org/abs/2601.17109) - Author credentials, institutional affiliation, peer review signals, and digital authority signals (domain trust, backlinks) each independently influence ChatGPT citation selection (Jacques et al., 2026); the authority signal framework generalizes to any domain where LLMs act as trust proxies, providing an actionable practitioner checklist.

- [Algorithmic Trust and Compliance in Regulated Industries](https://arxiv.org/abs/2603.12282) - In regulated sectors, compliance signals (regulatory licenses, standards body certifications) function as authority multipliers that LLMs treat as trust proxies (Oruesagasti, 2026); verifiable regulatory compliance displayed on-page is a high-leverage AEO signal in financial services, healthcare, and adjacent domains.

### Scale Audits & Brand Visibility

- [Generative Engine Optimization at Scale: Measuring Brand Visibility](https://arxiv.org/abs/2606.20065) - 100K+ AI search responses across 100+ brands mapping a three-tier visibility hierarchy (Kumar, 2026); best-of listicles account for ~21% of citations (highest of any format), YouTube and Reddit dominate non-corporate cited sources, brand maturity strongly predicts visibility tier, and sentiment framing fluctuates 6.7× more than mention frequency.

- [AI Brand Authority Index: 2.9 Million Brands by Associative Embeddedness in Gemini](https://dejan.ai/blog/brands/) - Graph-based brand visibility study: 200,000 recall surveys seed 5,892 brands, association queries grow the graph to 2.9 million nodes, Personalized PageRank with seed-weighted teleportation computes authority scores; key finding is that non-seed brands (not directly recalled) rank highest through "dense intersections of associations from high-authority seeds" rather than direct recall - establishing network centrality in brand association graphs as a measurable AEO signal distinct from direct name recall.

- [Prominence-Stratified Failure Modes](https://arxiv.org/abs/2605.27439) - 37,000-run audit across 215 commercial queries and 533 brands (Jack et al., 2026) showing L4/L5 brands are invisible in 48–52% of all runs while L1 category leaders win only 25–41% of available recommendation slots - optimization strategy must be calibrated to brand tier, not applied uniformly.

- [Generative Engine Optimization: How to Dominate AI Search](https://arxiv.org/abs/2509.08919) - Systematic large-scale study documenting an overwhelming earned-media bias across ChatGPT, Perplexity, and Gemini (Chen et al., 2025); third-party authoritative mentions far outperform owned content, and engine-specific and language-specific strategies are necessary because no single cross-platform approach transfers reliably.

- [The Discovery Gap: Product Hunt Startups in LLM Discovery](https://arxiv.org/abs/2601.00912) - LLMs recognize products by name at 99% but surface them in organic discovery queries at only 3–8% (Sharma, 2026); traditional SEO signals - referring domains and community rank - predicted Perplexity visibility better than any AI-specific tactic, making SEO foundation a prerequisite for LLM discoverability rather than an alternative to it.

### Causal Inference & Methodology

- [Disentangling AEO from Platform Growth](https://arxiv.org/abs/2606.04362) - Natural experiment using untreated pages as controls isolates true causal AEO effect from ChatGPT's underlying platform growth (Watanabe and Nakayashiki, 2026); genuine AEO lift was 1.82× ChatGPT referral (95% CI: 1.31–2.54, p=0.001) while site-wide ChatGPT traffic grew 5.7× from platform expansion alone - the canonical methodological warning for AEO attribution.

- [Quantifying Uncertainty in AI Visibility](https://arxiv.org/abs/2603.08924) - AI search citation distributions follow power laws with high variance, making single-run visibility metrics statistically unreliable for decision-making (Sielinski, 2026); bootstrap confidence intervals show many apparent performance differences fall within measurement noise, with minimum sample size guidance published for Perplexity, SearchGPT, and Gemini.

### Evaluation Frameworks and Methodology

- [HELM: Holistic Evaluation of Language Models](https://github.com/stanford-crfm/helm) - Stanford CRFM's open evaluation framework covering 100+ scenarios and 50+ metrics with calibrated uncertainty estimates; the methodological template for longitudinal citation rate studies and controlled AEO experiments requiring statistical rigor beyond single-run visibility scores.

- [A Survey of Cross-Lingual Word Embedding Models](https://arxiv.org/abs/1706.04902) - Sebastian Ruder's foundational survey of cross-lingual representation learning (Ruder et al., 2017); the methodology reference for researchers studying how citation probability varies across languages or regional engine deployments - the largest empirical gap in the current GEO literature.

- [IndicGEO](https://github.com/ifham001/IndicGEO) - English/Hinglish brand-mention and citation dataset with reproducible analysis of 959 responses from ten skincare and fashion prompts across two India-focused waves, using ChatGPT and Gemini UI captures plus Perplexity Sonar API.

- [Survey of Hallucination in Natural Language Generation](https://arxiv.org/abs/2202.03629) - Comprehensive taxonomy of hallucination types, detection methods, and mitigation strategies (Ji et al., 2022); explains the parametric knowledge threshold that determines when LLMs bypass live retrieval entirely, making this the mechanistic basis for targeting knowledge-gap content in AEO strategy.

- [Lil'Log: Extrinsic Hallucination in LLMs](https://lilianweng.github.io/posts/2024-07-07-hallucination/) - Lilian Weng's technical analysis of hallucination in production retrieval systems covering attribution failure modes, grounding methods, and retrieval-augmented mitigation patterns; directly explains the citation decay dynamics that occur as engine retrieval models update and parametric coverage of a topic expands.

### Emerging Optimization Surfaces

- [ALAS: Autonomous Learning Agent for Self-Updating Language Models](https://arxiv.org/abs/2508.15805) - Autonomous pipeline continuously fine-tuning LLMs on post-cutoff web content raises post-cutoff accuracy from 15% to 90% on evolving topics (Atreja, 2025); appearing as a cited source in continuous-learning pipelines - not just in live retrieval - is an emerging AEO surface where high-quality, stable, citable content gets embedded into model weights over time.

---

## AEO & GEO Fundamentals

Conceptual and architectural grounding for why AI search is different from keyword-based search. Read this section before the tooling and tactics sections.

- [GEO: Generative Engine Optimization](https://arxiv.org/abs/2311.09735) - The paper (Aggarwal et al., KDD 2024) that named the field and established the vocabulary: nine optimization strategies evaluated against the GEO-bench dataset showing up to 40% visibility gains, with statistics and cited sources performing best overall and domain-specific variation determining which strategies apply where.

- [C-SEO Bench: Does Conversational SEO Work?](https://arxiv.org/abs/2506.11097) - First systematic benchmark of conversational SEO methods across domains and competitive saturation scenarios (Puerto et al., NeurIPS 2025); most C-SEO methods are ineffective or actively harmful to ranking, and gains collapse to zero-sum as competitors converge on the same tactics - the required methodological counterweight before investing in any specific GEO tactic.

- [Is AEO Different to SEO, or Is It All One Big Grift?](https://discoveredlabs.com/blog/is-aeo-geo-different-to-seo) - Makes the architectural case that the distinction is real based on retrieval and synthesis mechanics rather than ranking mechanics, explaining why the same page can rank #1 organically while being invisible to AI Overviews.

- [Position: GEO Creates Underexamined Risks](https://arxiv.org/abs/2606.12439) - Formalizes three structural governance risks (Wen et al., ICML 2026 position track): concentrated influence from retrieval systems favoring established players, undisclosed commercial embedding in AI answers, and academic-industry measurement misalignment where controlled-setting benchmarks understate production deployment risks.

- [Retrieval-Augmented Generation for Large Language Models: A Survey](https://arxiv.org/abs/2312.10997) - Comprehensive survey of Naive, Advanced, and Modular RAG paradigms (Gao et al., 2023) covering chunking strategy, embedding similarity, reranking, and prompt injection - directly explains which content properties get a document retrieved and which get filtered before reaching the LLM context window.

- [Great Disappearance Acts: Generative Search and Shadow Banning](https://arxiv.org/abs/2606.11216) - Structural argument that AI search creates zero-click at a deeper level than featured snippets (Friedmann, 2026) - content is consumed by the synthesis layer without attribution traffic - shifting the appropriate success metric from referral visits to brand impression and citation presence.

- [Commercialized Generative AI: Ethics of Native Advertising in Conversational Search](https://arxiv.org/abs/2310.04892) - Demonstrates that LLMs can blend sponsored content into AI-generated answers indistinguishably from organic responses (Zelch et al., 2023); as paid inclusion enters AI answers, organic authority signals - earned media, established backlink profiles, factual recurrence - become the primary differentiators between paid and unpaid AI visibility.

---

## Content Engineering

Evidence-based frameworks for structuring and writing content that AI engines retrieve, absorb, and cite. Includes tooling for evaluation and optimization.

- [Answer Engine Optimization Playbook](https://discoveredlabs.com/blog/answer-engine-optimization-playbook-how-to-get-cited-2025) - Practical framework covering owned page optimization via the CITABLE framework, third-party mention building, competitor comparison positioning, and review site strategy - the operational translation of the citation research into content team workflows.

- [Reverse-Engineering the GPT-5 Tokenizer for AEO/GEO](https://metehan.ai/blog/reverse-engineering-the-gpt-5-tokenizer-aeo-geo) - Analysis of 200,019 o200k_base tokens reveals plain prose achieves 2.2x better retrieval density than markdown tables (5.9 chars/token vs. 2.7 chars/token), numerals outperform written-out words, common abbreviations outperform full names, and leading with answers maximizes context-window efficiency; directly explains why format choices that look equivalent to humans have measurably different citation outcomes inside the model's context window.

- [CITABLE: The AEO Content Framework for Getting B2B Brands Cited by AI](https://discoveredlabs.com/blog/citable-the-aeo-content-framework-we-use-to-get-b2b-brands-cited-by-ai) - Defines the seven-dimension CITABLE framework in implementation terms: Clear entity and structure (2–3 sentence lead definitions), Intent architecture (primary and adjacent query coverage), Third-party validation (Wikipedia, reviews, news citations), Answer grounding (verifiable facts with sources), Block-structured for RAG (200–400 word self-contained sections), Latest and consistent (timestamps and unified facts across pages), Entity graph and schema (explicit relationships via JSON-LD); the practitioner translation of the same framework scored by the AEO Content Evaluator tool.

- [Structural Feature Engineering for GEO (GEO-SFE)](https://arxiv.org/abs/2603.29979) - Hierarchical three-level content structure framework targeting macro (document architecture), meso (information chunking), and micro (visual emphasis) levels (Yu et al., 2026); validated 17.3% citation rate improvement and 18.5% subjective quality improvement across six generative engines, confirming structural optimization generalizes across engine architectures.

- [Think Before Writing: FeatGEO](https://arxiv.org/abs/2604.19113) - Optimizes document-level structural, content, and linguistic feature configurations before generating text rather than editing tokens post-hoc (Liu and Xu, 2026); document-level properties influence citation behavior more strongly than isolated word-level edits, and configurations generalize across LLM scales.

- [SAGEO Arena: Realistic Evaluation of Search-Augmented GEO](https://arxiv.org/abs/2602.12187) - End-to-end benchmark under realistic retrieval conditions showing that popular GEO tactics - keyword injection, prompt-style text additions - degrade performance at retrieval and reranking stages (Kim et al., 2026); structural web markup (schema.org) is one of the few signals that reliably survives the full pipeline, requiring stage-specific optimization strategies rather than document-level heuristics.

- [E-GEO: E-Commerce GEO Benchmark](https://arxiv.org/abs/2511.20867) - 7,000+ product queries with 15 rewriting heuristics evaluated against an iterative optimization algorithm (Bagga et al., 2025); a domain-agnostic stable content pattern emerges from iterative optimization that substantially outperforms all 15 heuristic baselines - the structured listing format generalizes beyond e-commerce.

- [Iterate Until Retrieved: Factual Nugget Optimization](https://arxiv.org/abs/2605.25641) - Iterative framework converting factual corrections into knowledge-base entries structured for reliable retrieval in production RAG pipelines (Hazoom et al., 2026); standard semantically correct entries are not reliably retrieved - iterative format and phrasing optimization substantially improves discoverability in real B2B RAG systems.

- [Retrieval Collapses When AI Pollutes the Web](https://arxiv.org/abs/2602.16136) - With 67% synthetic content in a retrieval pool, over 80% of surfaced results become AI-generated while accuracy appears stable (Yu et al., 2026); original, human-authored content with unique provenance signals becomes increasingly scarce and therefore prioritized by quality-aware rankers - differentiated content is a durable long-term AEO asset.

- [BibTeX Citation Hallucinations in Scientific Publishing Agents](https://arxiv.org/abs/2604.03159) - Even search-enabled LLMs default to parametric memory over live retrieval, producing a 27-point accuracy drop for recent vs. established content (Rao and Callison-Burch, 2026); longevity and accumulated authoritative back-references are stronger citation guarantors than recency for most topics, and structural unambiguity reduces hallucination risk.

- [Controlling Output Rankings in Generative Engines (CORE)](https://arxiv.org/abs/2602.03608) - 91.4% promotion success rate at Top-5 using content injection strategies across GPT-4o, Gemini, Claude, and Grok (Jin et al., 2026); included as a security warning - exposes the ranking manipulation vulnerability that will motivate counter-measures from engine developers against aggressive content injection tactics.

- [AEO Content Evaluator](https://discoveredlabs.com/tools/aeo-content-evaluator) - Free tool scoring pasted content (up to 15,000 characters) across seven CITABLE dimensions: Semantic Clarity, Citation & Answer Analysis, RAG Optimization, Third-party Validation, Intent Architecture, Freshness & Consistency, and AI Detection - outputs an overall readiness score, strength/weakness breakdown, and prioritized improvement recommendations with re-evaluation capability.

- [Heading Optimizer](https://discoveredlabs.com/tools/heading-optimizer) - Free tool generating 10+ headline variants ranked via pairwise tournament comparison using log probabilities, with beam search refinement producing a Pareto-optimal ranked list trading off AI citation preference against Flesch-Kincaid readability; evaluates query fan-outs and AGREE citation grounding in 30–60 seconds per run.

---

## Technical SEO

Implementation references for engineers configuring sites for AI crawler access, structured data coverage, and agent-compatible content serving.

- [Google AI Overviews: Official Indexing and Control Documentation](https://developers.google.com/search/docs/appearance/ai-overviews) - Authoritative on what engineers can and cannot control: AI Overviews use query fan-out across multiple parallel sub-searches; no special markup exists beyond standard Search eligibility; inclusion is controlled via `robots.txt`, `nosnippet`, `data-nosnippet`, `max-snippet`, and `noindex` directives.

- [Google: Creating Helpful, Reliable, People-First Content](https://developers.google.com/search/docs/fundamentals/creating-helpful-content) - Google's authoritative documentation on E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness); while the isolated contribution of E-E-A-T to AI Overview citation probability is an open empirical question, the authority signals it codifies - author credentials, institutional affiliation, factual accuracy, peer citation - appear consistently as citation predictors in empirical studies including the Authority Signals paper above.

- [WebMCP W3C Community Group Draft](https://github.com/webmachinelearning/webmcp) - Spec authored by Microsoft and Google defining `navigator.modelContext.registerTool()` as a browser-native JS API; zero infrastructure required - tools execute in page context sharing user auth. Status: CG-DRAFT with core API defined, security model under active development. The architectural reference for browser-side agent-compatible content exposure, complementing server-side llms.txt and MCP approaches.

- [Introduction to Structured Data for Google Search](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data) - Implementation guide covering JSON-LD (recommended), Microdata, and RDFa formats; required vs. recommended properties for rich result eligibility; and the validation toolchain (Rich Results Test, Rich Result Status Reports, URL Inspection Tool) - explicitly links structured data completeness to AI feature eligibility.

- [Schema.org Technical Documentation](https://schema.org/docs/documents.html) - Full type hierarchy (downloadable as a single file), data model, extension mechanism for custom vocabulary, and domain-specific vocabularies (health, automotive, financial services, hospitality) at V30.0 (2026-03-19) - the authoritative vocabulary reference underlying all Google structured data and AI search grounding.

- [llms-txt-hub](https://github.com/thedaviddias/llms-txt-hub) - Largest curated directory of llms.txt implementations (863 stars): searchable registry with generators, validators, and integration guides for Next.js, Docusaurus, Hugo, and Astro - the primary reference for the llms.txt standard.

- [dualmark](https://github.com/dodopayments/dualmark) - Dual-serves clean Markdown to AI bots and polished HTML to humans from the same URL via HTTP content negotiation (91 stars); six framework adapters (Next.js, Astro, SvelteKit, Cloudflare Workers, Deno Deploy, Vercel Edge), CLI conformance scorer (0–125 pts), and automatic llms.txt generation. Apache 2.0 with patent grant.

- [aeo.js](https://github.com/multivmlabs/aeo.js) - Framework-agnostic library (100 stars) generating llms.txt, robots.txt, sitemaps, and JSON-LD structured data with a browser-based AI-crawler visibility scorer (0–100) and a widget for toggling AI-readable markdown views; supports Astro, Next.js, Nuxt, Remix, SvelteKit, Angular, Vite, and plain CLI. MIT.

- [MCP vs RAG vs NLWeb vs HTML](https://arxiv.org/abs/2511.23281) - RAG outperforms raw HTML, API endpoints, and natural-language web interfaces for AI agent retrieval on both accuracy and efficiency (Steiner et al., 2025); how content is exposed to agents matters as much as the content itself - the architectural decision guide for structuring content exposure for AI agent discoverability.

- [GPTBot Crawled 60,000 Pages 30,000+ Times in 12 Hours](https://metehan.ai/blog/i-built-a-60-000-page-ai-website-for-10-gptbot-crawled-it-30-000-times-in-12-hours) - Controlled experiment on a new domain with zero backlinks: GPTBot made 29,000+ requests at approximately one per second within 12 hours - a 470x higher crawl volume than Googlebot over the same period; confirms AI crawlers are extremely aggressive at discovering and processing new content regardless of domain authority, making crawl budget management a distinct engineering concern from traditional SEO.

- [Crawling 65,000 Pages Without Parsing HTML: HTTP Headers as LLM Crawler Targets](https://metehan.ai/blog/http-headers-internal-links) - Encoding internal link graphs and heading hierarchies as base64url-compressed JSON directly in HTTP response headers (`X-Internal-Links`, `X-Headings`) achieves 8.6x faster crawl speeds with edge caching versus HTML parsing; revealed 41.9% of pages were orphaned (unreachable via internal navigation) - a site architecture problem invisible to traditional crawlers that LLM crawlers are disproportionately exposed to.

- [Agentic Browsing Checker](https://discoveredlabs.com/tools/agentic-browsing-checker) - Free tool running 19 checks across the OPERABLE framework's eight pillars: WebMCP tool registration, form annotation coverage, accessibility tree quality, llms.txt validation, ARIA live region coverage, button keyboard operability, and AI crawler robots.txt access; uses Puppeteer, Lighthouse, and WebMCP W3C draft compliance testing, delivers a scored report by email in 45–90 seconds.

- [AI Assist Widget](https://discoveredlabs.com/tools/ai-assist-widget) - Free tool: single script tag (<5KB minified, zero external dependencies, CSS scoped with unique IDs) that adds a floating bar of buttons opening ChatGPT, Claude, Perplexity, Grok, or Gemini in a new tab with the current page URL pre-filled in the prompt; configurable engine selection, position, colors, labels, and default prompt; no tracking, analytics, or external requests - runs entirely in-browser; each visitor interaction exposes the page to an AI system that processes and learns from the content.

---

## Measurement & Evaluation

Statistical frameworks and tooling for building AEO measurement systems that distinguish signal from noise - the highest-stakes section for teams making investment decisions based on AI visibility data.

- [Quantifying Uncertainty in AI Visibility](https://arxiv.org/abs/2603.08924) - AI search citation distributions follow power laws with high variance, making single-run visibility metrics statistically unreliable for decision-making (Sielinski, 2026); bootstrap confidence intervals show many apparent performance differences fall within measurement noise, with minimum sample size guidance for Perplexity, SearchGPT, and Gemini.

- [Why Most AEO Tools Give You Noise](https://discoveredlabs.com/blog/aeo-test-bed-noise-vs-signal) - Practical companion to the Sielinski paper defining what rigorous AEO measurement requires: credible intervals, multi-run consistency checks, and explicit separation of between-question variance (different prompts yielding 60% vs. 20% mention rates) from within-question variance (LLM stochasticity across runs of the same prompt).

- [LLM Eval Calculator](https://discoveredlabs.com/tools/llm-eval-calculator) - Free tool calculating statistically required sample sizes for measuring brand mention rates across AI platforms; accounts for both between-question and within-question variance and outputs unique prompts required, total API calls, and time estimates - at 95% confidence, ±2% margin, K=3 resamples: 353 unique prompts × 3 = 1,059 total API calls.

- [Disentangling AEO from Platform Growth](https://arxiv.org/abs/2606.04362) - (Cross-listed from [Research & Papers](#research--papers).) Methodological template for natural experiments isolating true causal AEO effect from platform-level growth; genuine lift was 1.82× while raw site-wide ChatGPT traffic grew 5.7× from platform expansion alone.

- [16 Months of Google Search Console Data as a Vector Database](https://metehan.ai/blog/i-turned-16-months-of-google-search-console-data-into-a-vector-database-heres-what-i-learned) - GSC performance data embedded via Gemini and stored in ChromaDB enables semantic query discovery (related queries without exact keyword matching) that SQL cannot surface; concludes the optimal measurement stack combines SQL for exact metric queries with a vector DB for exploratory gap analysis - directly applicable to building AEO measurement infrastructure on top of existing search data.

- [Why AI Visibility Trackers Are Not Measuring What You Think They Are](https://discoveredlabs.com/blog/ai-tracking-platforms-test-flaw) - Identifies a fundamental measurement flaw across most AI visibility platforms: incognito mode testing lacks the web search tools available to 99% of logged-in ChatGPT users, creating a measurable divergence where domains showing 0% citation rate in incognito testing show 5–8% under real user conditions; includes reverse-engineered tool capability specifications from ChatGPT's own API responses and an analysis of OpenAI's SerpAPI cost structure driving the discrepancy.

- [The Scientific Method for AEO: How to Model, Test, and Win AI Search](https://discoveredlabs.com/blog/how-to-model-test-win-ai-search-for-your-brand) - Presents a controlled single-variable AEO testing framework: intelligent resampling to reduce LLM variance, clustered standard errors for related prompts, and power analysis for proper sample sizing; tests isolate content structure, authority signals, and entity relationships across 100+ prompts with multiple reruns to achieve statistical significance - the practitioner implementation of the methodology described in the academic papers above.

- [Reddit & ChatGPT Influence 2025](https://discoveredlabs.com/research/reddit-chatgpt-influence-2025) - 144,284 AI citations across ChatGPT, Google AI Overviews, and Gemini collected November–December 2025, with traffic interception used to derive RRF slot allocation independently of visible citations; the methodology section is a reusable template for reverse-engineering how specific engines weight any source type.

- [Discovered Labs: AI Visibility Tracker](https://discoveredlabs.com/technology/ai-visibility-tracker) - Managed monitoring platform testing real buyer questions (sourced from Google Search Console, support tickets, call transcripts, and subreddits) across ChatGPT, Claude, Perplexity, Google AI Overviews, and Gemini every five days; tracks citation rate, mention rate, average ranking position, share of voice, citation source breakdown (Reddit, reviews, YouTube, blogs), brand perception scoring, and competitor benchmarking; statistically rigorous - each prompt runs multiple times with 95% credible intervals, and movements only register after three consecutive significant runs to eliminate LLM stochasticity noise.

---

## Reddit & Off-Page Signals

Reddit is a high-leverage AEO surface that most technical teams underinvest in because the relationship between retrieval usage and visible attribution is counterintuitive. Evidence and tooling for understanding and acting on Reddit as an AI citation signal.

- [Reddit & ChatGPT Influence 2025](https://discoveredlabs.com/research/reddit-chatgpt-influence-2025) - 28,633 ChatGPT citations analyzed: Reddit occupies ~27% of ChatGPT's RRF retrieval allocation (~16 of ~60 slots) yet appears in only 0.35% of visible citations - a 99% gap between retrieval usage and visible attribution; Google cites Reddit at 2.11%, Gemini at 0.99%, making the inter-engine attribution gap a strategically actionable signal.

- [Reddit Threads Finder](https://discoveredlabs.com/tools/reddit-threads-finder) - Free tool identifying Reddit threads currently cited in ChatGPT and Gemini responses for a given domain: analyzes your site to infer buyer questions, runs those through ChatGPT and Gemini, scores each thread on opportunity score (AI visibility potential) and relevance score (commercial intent, engagement metrics, recency, AI visibility gap), and returns a subreddit-level breakdown within 1–2 minutes.

- [Generative Engine Optimization at Scale: Measuring Brand Visibility](https://arxiv.org/abs/2606.20065) - (Cross-listed from [Research & Papers](#research--papers).) YouTube and Reddit are the top non-corporate cited sources; best-of listicle thread format accounts for ~21% of citations - directly informing which thread types to create or participate in.

- [Generative Engine Optimization: How to Dominate AI Search](https://arxiv.org/abs/2509.08919) - (Cross-listed from [Research & Papers](#research--papers).) Earned-media framework: high-engagement Reddit threads with authoritative contributors qualify as earned media, which far outperforms owned content across all tested engines.

- [One Polluted Page Is Enough](https://arxiv.org/abs/2606.13610) - A single strategically placed document in the retrieval corpus steers AI recommendations across all tested models (Luo and Chen, 2026); the corollary for legitimate AEO: one high-authority third-party mention on a well-ranked domain can meaningfully lift brand surfacing in AI-generated recommendations.

> **Research gap:** No peer-reviewed paper exists specifically analyzing Reddit as a controlled AEO optimization surface. The Discovered Labs research above is the primary empirical source for this section.

---

## Agentic SEO

AI agents - coding assistants, browser agents, autonomous research systems - are a search surface architecturally distinct from conversational AI. Agentic retrieval requires different infrastructure and content choices than traditional AI search optimization.

Key differences from conversational AEO:

- Agents generate their own queries from internal hypotheses, not from user queries passed directly to retrieval ([LiveBrowseComp](https://arxiv.org/abs/2605.28721)): ~45% of agent answers bypass search entirely; content must match the model's internal framing of a topic.
- Agentic rerankers are evolving beyond relevance scores to emit contribution summaries and evidence passages ([Prism-Reranker](https://arxiv.org/abs/2604.23734)) - content that explicitly articulates its distinctive value is preferred over content that only covers the topic.
- How content is exposed to agents matters as much as content quality - RAG-compatible APIs and semantic feeds outperform plain HTML for agent discoverability ([MCP vs RAG vs NLWeb vs HTML](https://arxiv.org/abs/2511.23281)).
- Site architecture can be designed to guide agent navigation trajectories across pages, not just to optimize individual documents ([EcoGEO](https://arxiv.org/abs/2605.12887)).

Discovered Labs operates an MCP server providing AEO-specific workflows - prompt hygiene, citation auditing, Reddit opportunity scoring, and AI engine querying - available inside Claude Desktop and Cursor. No public repository.

For a curated list of agentic GEO frameworks, MCP servers for SEO data, agent skill packages, and autonomous content pipelines, see [awesome-aeo-seo-agents](https://github.com/discoveredlabs/awesome-aeo-seo-agents).

---

## Open Source Tools

GitHub repositories with runnable code. Stars noted as of curation date. Vendor names appear in descriptions where relevant; no vendor URLs are linked.

### Audit & Optimization

- [geo-optimizer-skill](https://github.com/Auriti-Labs/geo-optimizer-skill) - 47 research-backed checks across 8 scoring categories (0–100), querying real engines (ChatGPT, Perplexity, Gemini, Google AI Overviews, Claude) to verify actual citations; auto-generates llms.txt, JSON-LD schema, and robots.txt rules with CLI, Python library, MCP server, Astro integration, and CI/CD support. 502 stars, Python.

- [AutoGEO](https://github.com/cxcscmu/AutoGEO) - ICLR 2026 implementation automatically extracting content preferences from generative engines and rewriting documents to improve AI citation rates; two modes: AutoGEOAPI (prompt-based) and AutoGEOMini (RL-trained), evaluated with GEO visibility score and GEU utility preservation metric. 177 stars, Python.

- [GEO](https://github.com/GEO-optim/GEO) - Reference implementation from the KDD 2024 foundational paper: GEO-BENCH dataset, custom optimization function framework, and leaderboard - the empirical baseline against which subsequent GEO claims should be validated. 293 stars, Python.

- [agentic-seo](https://github.com/addyosmani/agentic-seo) - CLI audit for agentic engine optimization: 10 checks across 5 categories (Discovery, Content Structure, Token Economics, Capability Signaling, UX Bridge) scored 0–100; scaffolds missing AEO files (llms.txt, AGENTS.md, skill.md); supports Next.js, Astro, Hugo, and Docusaurus. 244 stars, JavaScript.

- [seobuild-onpage](https://github.com/gbessoni/seobuild-onpage) - Full page generation pipeline: pulls live SERP data, analyzes top competitors, extracts People Also Ask, finds content gaps, and writes structured pages with FAQ sections and schema markup; includes a 55-point post-generation quality checklist targeting Google AI Overviews and Perplexity explicitly. 218 stars, Python. MIT.

- [claude-seo](https://github.com/AgriciDaniel/claude-seo) - Universal SEO agent skill for Claude Code: 25+ sub-skills and 18 specialist agents covering technical SEO, E-E-A-T, Schema.org (30+ types), GEO/AEO via `/seo geo` sub-skill, backlink analysis, local SEO, semantic clustering, and e-commerce; SPA-aware via Playwright with optional Firecrawl and DataForSEO MCP extensions. 9,900 stars, Python. MIT.

### AI Visibility Monitoring

- [elmo](https://github.com/elmohq/elmo) - Self-hosted visibility monitor tracking ChatGPT, Claude, and Perplexity with competitor benchmarking and full data ownership; stack: Docker Compose + PostgreSQL + TanStack Start; actively maintained through v0.2.13. 148 stars, TypeScript. MIT.

- [canonry](https://github.com/Canonry/canonry) - Self-hostable AEO monitoring platform tracking citations across Gemini, ChatGPT, Claude, and Perplexity; ingests server logs from Cloud Run, Vercel, and WordPress; integrates Google Search Console, GA4, and Bing Webmaster Tools; discovers backlinks via Common Crawl using local DuckDB; 67-tool MCP adapter for Claude Desktop. 71 stars, TypeScript. FSL-1.1-ALv2 (converts to Apache 2.0 after 2 years).

- [ai-brand-monitor-mcp](https://github.com/khadinakbarlabs/ai-brand-monitor-mcp) - MCP server tracking brand visibility across Perplexity, ChatGPT, Claude, and Gemini with domain citation detection, competitor co-mention analysis, and a 0–100 GEO visibility score with actionable recommendations; deployable inside any MCP-compatible coding assistant or agent pipeline. 56 stars, TypeScript. MIT.

- [ansvisor](https://github.com/ansvisor/ansvisor) - Self-hosted citation tracker (Next.js 16 + Express + PostgreSQL) monitoring 8+ AI engines with weekly trend dashboards, topic/intent clustering, URL-level citation analytics with geographic breakdown, and AI-generated content briefs with CMS webhook delivery. 36 stars, TypeScript. MIT.

### Technical Infrastructure

- [llms-txt-hub](https://github.com/thedaviddias/llms-txt-hub) - Largest curated directory of llms.txt implementations (863 stars): searchable registry with generators, validators, and integration guides for Next.js, Docusaurus, Hugo, and Astro - the primary reference for the llms.txt standard.

- [dualmark](https://github.com/dodopayments/dualmark) - Dual-serves clean Markdown to AI bots and polished HTML to humans from the same URL via HTTP content negotiation; six framework adapters, CLI conformance scorer (0–125 pts), and Apache 2.0 with patent grant. 91 stars, TypeScript.

- [aeo.js](https://github.com/multivmlabs/aeo.js) - Framework-agnostic library generating llms.txt, robots.txt, sitemaps, and JSON-LD structured data with a browser-based AI-crawler visibility scorer and markdown widget; supports Astro, Next.js, Nuxt, Remix, SvelteKit, Angular, Vite, and CLI. 100 stars, TypeScript. MIT.

- [agent-seo](https://github.com/pontiggia/agent-seo) - Middleware detecting 17+ AI crawler user agents (GPTBot, ClaudeBot, and others) and serving clean Markdown instead of HTML; adapters for Next.js, Express, Fastify, and Hono - the server-side content negotiation option for server-rendered frameworks without CDN transform support.

### Data & Research

- [open-seo](https://github.com/every-app/open-seo) - Open-source SEO data platform: keyword research, rank tracking, competitor analysis, backlink analysis, and site audits with an MCP interface for AI agent workflows; self-hostable via Docker or Cloudflare with optional Google Search Console integration. 3,100 stars, TypeScript. MIT.

- [firecrawl](https://github.com/firecrawl/firecrawl) - Converts any URL into clean LLM-ready Markdown or structured data with dynamic page support, batch crawling, and site mapping - the de facto standard infrastructure layer for competitive content research, knowledge base construction, and SERP data extraction in multi-agent SEO pipelines. 30,000+ stars, TypeScript.

- [gpt-researcher](https://github.com/assafelovic/gpt-researcher) - Autonomous research agent that plans, searches, scrapes, and synthesizes reports with citations; most relevant for competitive intelligence, content gap analysis, and building the type of citation-worthy original research that the citation data shows dominates AI engine outputs. 27,900 stars, Python.

---

## Related Lists

- [awesome-aeo-seo-tools](https://github.com/discoveredlabs/awesome-aeo-seo-tools) - Discovered Labs curated list of free and open-source tools for AEO/GEO auditing, monitoring, content optimization, MCP servers for SEO data, and agent skill packages - deeper tool coverage than this list.
- [awesome-aeo-seo-agents](https://github.com/discoveredlabs/awesome-aeo-seo-agents) - Discovered Labs curated list covering agentic GEO frameworks, MCP servers for SEO/AEO data, agent skill packages, content generation pipelines, and agentic browsing compatibility resources - the companion for engineering teams building agent-native SEO systems.
- [awesome-generative-engine-optimization](https://github.com/amplifying-ai/awesome-generative-engine-optimization) - Community-maintained list of GEO resources covering papers, tools, and articles.
- [Awesome-GEO](https://github.com/DavidHuji/Awesome-GEO) - Academic-focused curated list of GEO papers organized by topic.

---

## People to Follow

Researchers, educators, and practitioners whose work is most directly relevant to understanding LLM retrieval, evaluation, and agentic systems - the foundation for technical AEO work.

### Researchers and Educators

- [Elvis Saravia - DAIR.AI](https://dair.ai) - Founder of Democratizing AI Research; publishes the most widely used practical guides on prompt engineering, LLM evaluation, and RAG - required reading before building any AEO measurement or optimization pipeline.
- [Lilian Weng - Lil'Log](https://lilianweng.github.io) - OpenAI research blog covering RAG architectures, agent planning, emergent LLM behaviors, and RL foundations; the best single source for understanding the systems AEO optimizes for at a mechanistic level.
- [Sebastian Ruder](https://ruder.io) - Google DeepMind; multilingual NLP, cross-lingual transfer, and NLP progress tracking; the primary research reference for the multilingual AEO gap.
- [Chip Huyen](https://huyenchip.com) - Stanford CS329S instructor; ML systems design, production LLM deployment, and evaluation methodology; practical experimental design for engineers building AEO measurement systems.
- [Eugene Yan](https://eugeneyan.com) - Applied ML at Amazon; information retrieval, recommendation systems, and LLM evaluation; most relevant for understanding retrieval decay dynamics and content quality signals over time.
- [Jay Alammar](https://jalammar.github.io) - Visualizations of transformer internals, attention mechanisms, and embedding space behavior; builds the intuition for why content structure influences what models retrieve and absorb.
- [Andrej Karpathy](https://karpathy.ai) - LLM architecture, tokenization, and model internals; most relevant for understanding the parametric knowledge boundary that determines when live-web AEO matters at all versus when content is already embedded in weights.

### University Labs

- [Stanford CRFM - Percy Liang](https://crfm.stanford.edu) - Center for Research on Foundation Models; publishes HELM (holistic LLM evaluation) and foundation model transparency research - the methodological reference for longitudinal AEO measurement and controlled evaluation design.
- [CMU Language Technologies Institute - Graham Neubig](http://www.phontron.com) - Multilingual NLP, RAG systems, and query routing research; CS11-711 Advanced NLP is free online and covers embedding evaluation and retrieval methodology directly applicable to AEO measurement.
- [MIT CSAIL - Jacob Andreas](https://people.csail.mit.edu/jda/) - Language grounding, long-context retrieval, and compositional semantics; most relevant to the long-context chunking and retrieval optimization gap.

### Courses

- [Stanford CS224N: NLP with Deep Learning](https://web.stanford.edu/class/cs224n/) - Christopher Manning; the foundational NLP course covering retrieval, embeddings, and language model architecture - prerequisite for understanding GEO at a mechanistic level.
- [Stanford CS329S: ML Systems Design](https://stanford-cs329s.github.io) - Chip Huyen; experimental design, evaluation pipelines, and production ML - directly applicable to building rigorous AEO measurement infrastructure.
- [Stanford CS25: Transformers United](https://web.stanford.edu/class/cs25/) - Free lecture series on transformer architecture, multimodal models, and emerging LLM applications; available on YouTube.
- [MIT 6.S191: Introduction to Deep Learning](http://introtodeeplearning.com) - Alexander Amini; covers neural networks, retrieval, and multimodal learning - the shortest path to understanding AI search retrieval mechanics from first principles.
- [CMU CS11-711: Advanced NLP](https://phontron.com/class/anlp2025/) - Graham Neubig; free online; covers RAG, embedding evaluation, and multilingual NLP - the graduate-level reference for AEO measurement methodology.
- [Deeplearning.ai: AI Agents in LangGraph](https://www.deeplearning.ai/courses/ai-agents-in-langgraph/) - Andrew Ng and Harrison Chase; multi-agent architectures, tool use, and agentic pipeline design - the practical foundation for building agentic AEO pipelines.

---

## Contributing

See [contributing.md](contributing.md) for submission guidelines and open research problems this list is looking to fill.

---

## License

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [Discovered Labs](https://discoveredlabs.com) has waived all copyright and related rights to this work.
