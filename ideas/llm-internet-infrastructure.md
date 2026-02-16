# The Internet's New Customer: How LLMs Should Reshape Data Infrastructure

The internet was built for humans clicking links. That era is ending.

Today, the primary consumer of web data is not a person with a browser. It's a language model with a context window. Every layer of the stack — from HTML to auth — was designed for a customer that no longer dominates. The architecture is wrong. Not slightly wrong. Fundamentally wrong.

## What's Broken

Start at the bottom. HTML is a rendering format. It tells browsers where to put boxes and text. An LLM doesn't need boxes. It needs meaning. Yet we force models to parse `<div class="article-body">` and hope the CSS classes are semantic. They rarely are.

APIs should help, but they don't. Every service invents its own schema, its own auth flow, its own pagination. Connecting to ten APIs means learning ten languages. CRUD endpoints assume you know what you want before you ask. Models don't work that way. They reason toward an answer.

Databases store rows and columns. Models need vectors and relationships. Search engines rank by clicks, ads, and backlinks — proxies for relevance that served humans well enough. Models need truth, not popularity.

Authentication assumes a browser with cookies. CAPTCHAs assume a human with eyes. JavaScript rendering assumes a client that executes code. None of these assumptions hold when your customer is a transformer running on a GPU cluster.

## What LLMs Actually Need

The list is short. Clean text, structured for meaning. Consistent schemas across sources. Content sized for context windows — not paginated for screens. Semantic access: ask a question, get an answer, with provenance. No rendering. No scraping. No guessing.

This isn't exotic. It's simple. We just never built for it.

## The New Stack

**Formats.** Structured markdown with metadata. JSON-LD for semantic relationships. The `llms.txt` movement points the right direction: a machine-readable manifest at every domain root. Universal schemas — like Schema.org, but enforced, not optional.

**Databases.** The relational model doesn't die. It merges. Vector search for semantics. SQL for structure. Hybrid stores that serve both RAG pipelines and traditional queries. The database becomes retrieval-native — built to answer questions, not just return rows.

**Protocols.** HTTP needs new headers: `Accept: application/llm+json`. A `robots.txt` successor that declares access tiers, pricing, and usage rights for AI consumers. MCP — Anthropic's Model Context Protocol — and OpenAI's function calling point toward a standard: tool-use as the universal API. One interface. Every service. The model calls functions; the protocol handles the rest.

**Pricing.** Per-page and per-seat pricing assumed human consumption rates. The new model is per-token. You pay for what you read. Data providers become API-first businesses, metered for machine throughput.

## Who Wins

Whoever controls the format and access layer captures the value. This is the "Internet Reclaimed" thesis: if you own your data in the right structure, you serve both humans and machines as a first-class citizen. If you don't, you're scraped, summarized, and disintermediated.

The winners aren't just big platforms. They're anyone who publishes structured, machine-readable data with clear access terms. A personal blog with `llms.txt` and clean markdown has more value to an LLM than a JavaScript-heavy news site behind a CAPTCHA wall.

## What Exists Already

This isn't theoretical. MCP defines tool-use protocols for models. OpenAI and Anthropic ship structured outputs and function calling. The `llms.txt` movement grows weekly. Vector databases — Pinecone, Weaviate, pgvector — are production infrastructure. RAG is standard practice.

The pieces exist. The stack doesn't. No one has assembled the full layer cake: format → protocol → database → access control → pricing. That's the work ahead.

The internet got its first new customer in thirty years. Time to rebuild the store.
