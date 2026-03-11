# How Machines Understand Meaning

March 2026

---

A search through 33,000 personal emails for "that conversation about moving to Portland" returns nothing useful in Gmail. The phrase never appeared in any message. But the underlying discussion did happen, scattered across threads about housing costs, school districts, and whether the rain was really that bad. A keyword search has no way to connect these things. It looks for the words provided and finds none of them in the relevant messages.

Vector embeddings solve this problem. The core idea is disarmingly simple: convert text into a list of numbers, arranged so that similar meanings end up near each other. The phrase "relocating to the Pacific Northwest" and "moving to Portland" would sit close together in this numerical space, despite sharing almost no words. This is not metaphor. It is geometry.

## Text as coordinates

A vector embedding is a point in high-dimensional space. Where a GPS coordinate uses two numbers (latitude and longitude) to fix a location on Earth, an embedding model uses hundreds or thousands of numbers to fix the location of a piece of text in a meaning space. The model that produced the email search above, for instance, outputs 768 numbers for each passage of text.

These numbers are not hand-chosen. Nobody decided that dimension 247 should represent "sentiment" or that dimension 583 should capture "geographic relevance." The dimensions emerge from training, and most do not correspond to any single human-interpretable concept. What matters is the relationships between points. Text about dogs and text about puppies land near each other. Text about astrophysics lands far away from both.

The distance between two points can be measured in several ways, but the most common is cosine similarity, which looks at the angle between two vectors rather than the raw distance. Two vectors pointing in nearly the same direction get a score close to 1. Two vectors pointing in unrelated directions score near 0. This measure turns out to be more robust than simple distance for high-dimensional data, where raw distances tend to lose their discriminative power.

## How the models learn

The earliest influential work was Word2Vec, published by Tomas Mikolov and colleagues at Google in 2013. Word2Vec trained on a deceptively simple task: given a word, predict the words around it (or vice versa). The key insight was that this prediction task forced the model to learn representations where words used in similar contexts ended up with similar vectors. The famous demonstration was arithmetic with words: the vector for "king" minus "man" plus "woman" landed near "queen."

Word2Vec operated on individual words. It had no way to represent phrases or sentences, and it could not handle words with multiple meanings. The word "bank" got a single vector whether it referred to a riverbank or a financial institution.

BERT, introduced by Google in 2018, changed this. BERT was trained by masking random words in sentences and learning to predict them from context. A sentence like "The [MASK] deposited sediment along the riverbank" would push the model to learn that the missing word was probably "flood" or "river," absorbing contextual information that Word2Vec could not capture. This approach, called masked language modeling, produced representations that were sensitive to the surrounding text. The word "bank" now got different vectors in different sentences.

Modern embedding models build on BERT's architecture but add a crucial training step: contrastive learning. The idea is to take pairs of texts that are known to be related (a question and its answer, a document and its summary) and train the model to place them near each other while pushing unrelated pairs apart. The training data for this step often comes from naturally occurring pairs: Wikipedia articles and their titles, search queries and the pages people clicked on, questions posted on forums and the accepted answers.

The hardest and most important refinement is hard negative mining. Early in training, the model learns easy distinctions. A passage about neurosurgery is obviously different from one about Italian cooking. The challenging cases are passages that share surface-level similarity but differ in meaning. "How to treat a python bite" and "How to train a Python developer" share vocabulary but address entirely different topics. Hard negative mining specifically seeks out these confusing pairs and forces the model to learn the distinction. This stage is what separates adequate embedding models from excellent ones.

## How search works

Once a model is trained, search becomes a geometric operation. Every document in a collection is passed through the model to produce a vector. These vectors are stored in an index. When a query arrives, it too gets converted to a vector, and the system finds the stored vectors closest to it.

For small collections (tens of thousands of documents), an exact search through every vector is fast enough. For larger collections, approximate nearest neighbor algorithms like HNSW (Hierarchical Navigable Small World) build graph structures that allow the search to skip most of the collection and still find the closest matches with high accuracy. These algorithms trade a small amount of precision for enormous speed gains, making it practical to search through millions of vectors in milliseconds.

The results are ranked by cosine similarity. A search for "difficulties with remote team communication" might return documents about "challenges coordinating across time zones," "Slack fatigue in distributed teams," and "video call burnout," none of which necessarily contain any of the original search terms. This is the fundamental advantage over keyword search: the system operates on meaning, not string matching.

## Why this matters

Traditional keyword search relies on an assumption that the words in a query will appear in the relevant documents. This assumption fails routinely. People describe the same concept in different words. Technical jargon varies across fields. Acronyms, synonyms, and idiomatic expressions all break the keyword model.

Full-text search engines like Elasticsearch have added layers of sophistication over the years: stemming, synonym expansion, TF-IDF weighting, BM25 scoring. These help, but they remain fundamentally bound to lexical matching. They cannot understand that "affordable housing crisis" and "people can't afford rent" are about the same thing.

Vector search handles this naturally. The two phrases, despite minimal word overlap, would land in similar regions of the embedding space. More subtly, vector search can capture conceptual relationships that no synonym list would cover. A search for "environmental impact of fast fashion" might surface a document about "textile waste in landfills" because the model has learned, from training on millions of text pairs, that these concepts are related.

The limitation is worth noting. Vector search can sometimes retrieve text that is topically related but not actually responsive to the query. It can also struggle with precise factual lookups where keyword matching excels: searching for a specific error code, a product SKU, or a person's exact name. The best modern search systems use both approaches, combining vector similarity with keyword matching to capture both semantic relevance and lexical precision.

## A practical example

The email search described at the opening of this essay is a real system. It runs on a Mac Mini with an M-series chip, using an open-source embedding model (nomic-embed-text, 768 dimensions) and a local vector database. The 33,000 emails were exported from Gmail, chunked into passages, and embedded in about forty minutes. The resulting index occupies roughly 200 megabytes of disk space.

Searching this index for "that time I had issues with my landlord" returns email threads about a security deposit dispute, a maintenance request that went unanswered, and a thread where a friend described a lease renewal negotiation. None of these threads contain the word "landlord." Several do not contain the word "issues." But they are all relevant, because the embedding model placed them near the query in meaning space.

The entire system runs locally. No data leaves the machine. The emails are never sent to an external API. This is a meaningful property for personal data: the same semantic search capability that powers large commercial systems can now run on consumer hardware, using models that are freely available and small enough to fit in memory.

## The trajectory

The field has moved remarkably fast. Word2Vec in 2013 showed that neural networks could learn meaningful representations of words. BERT in 2018 showed that context-dependent representations were both possible and dramatically better. The years since have produced a proliferation of specialized embedding models, each optimized for particular tasks: code search, legal document retrieval, multilingual text, image-text matching.

The models have also gotten smaller and more efficient. The original BERT had 110 million parameters. Current embedding models achieve better performance on retrieval benchmarks with similar or fewer parameters, thanks to improvements in training methodology rather than raw scale. This efficiency is what makes local deployment practical.

What has not changed is the core geometric intuition. Text goes in. Numbers come out. Similar meanings cluster together. Everything else, from the training procedures to the search algorithms to the applications, is refinement of this one idea. It is, in the end, a way of teaching machines that words are not just sequences of characters but carriers of meaning, and that meaning has a shape.
