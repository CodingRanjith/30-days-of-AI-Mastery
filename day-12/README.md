# Day 12 - Similarity Search

![Day 12 - Similarity Search](day%2012.png)

**Theme:** Week 2 - Retrieval, Search, and Knowledge Grounding  
**Date:** 2026-04-12

---

## Learning Objectives

By the end of Day 12, you should be able to:

- Explain how similarity search works from query embedding to ranked results.
- Distinguish top-k retrieval strategies and when to use thresholds or dynamic k.
- Design hybrid similarity search combining semantic and lexical signals.
- Evaluate retrieval quality with practical metrics like Recall@k and MRR.
- Improve result quality with reranking, filtering, and query rewriting.

---

## What Is Similarity Search?

Similarity search finds items that are "closest in meaning" to a query rather than exact text matches.

In embedding-based systems:

- Query text is converted to a vector.
- Candidate documents/chunks are already stored as vectors.
- A similarity metric (cosine/dot/L2) ranks candidates by closeness.

This is the engine behind semantic document retrieval, recommendations, and many RAG pipelines.

---

## Core Retrieval Flow

A typical similarity search request follows this path:

1. Accept user query.
2. Normalize/preprocess query text.
3. Generate query embedding.
4. Run vector search (exact or ANN) for top-k candidates.
5. Apply metadata filters and business constraints.
6. Optionally rerank candidates.
7. Return best matches (or pass to LLM in RAG systems).

If any step is weak, overall quality drops - retrieval is a chain, not a single function.

---

## Similarity Metrics and Their Behavior

### Cosine similarity

- Measures angle between vectors.
- Common default for normalized embeddings.

### Dot product

- Efficient in many vector DBs and ANN engines.
- Sensitive to vector magnitude unless embeddings are normalized.

### Euclidean distance (L2)

- Measures geometric distance directly.
- Can work well but depends on embedding space behavior.

Rule: use the metric recommended for your embedding model, and keep it consistent across indexing and query serving.

---

## Choosing Top-k, Thresholds, and Dynamic Retrieval

### Fixed top-k

- Simple and common baseline.
- Risk: too low misses context; too high adds noise.

### Similarity threshold

- Returns only results above a score cutoff.
- Useful when you prefer "no answer" over weak matches.

### Dynamic k

- Adjusts number of results based on score distribution or query type.
- Better balance for mixed workloads.

Production systems often combine top-k with a minimum similarity threshold.

---

## Hybrid Similarity Search (Semantic + Lexical)

Semantic search is strong for intent and paraphrase.  
Lexical search is strong for exact terms, IDs, and rare tokens.

Hybrid strategy:

- Retrieve from both systems.
- Normalize scores.
- Merge and rerank.

This typically improves both recall and precision, especially in enterprise datasets with jargon and identifiers.

---

## Improving Similarity Search Quality

Quality levers that often matter most:

- Better chunking (coherent chunk boundaries + overlap).
- Metadata filters (source, date, tenant, ACL).
- Query rewriting (expand abbreviations, normalize intent).
- Multi-query retrieval (ask multiple rewritten variants).
- Reranking top candidates with a stronger relevance model.

Often, these improvements beat blindly switching to a larger embedding model.

---

## Evaluation: Measure Retrieval, Not Just Final Answers

Useful retrieval metrics:

- Recall@k: did we retrieve any relevant item in top-k?
- Precision@k: how many returned items are relevant?
- MRR: how early does the first relevant result appear?
- NDCG: ranking quality with graded relevance.

Evaluation workflow:

- Build a query set with known relevant chunks/documents.
- Run baseline retrieval.
- Change one variable at a time.
- Compare metrics before/after.

Without retrieval metrics, RAG debugging becomes guesswork.

---

## Failure Modes and Debugging Checklist

- Noisy top results -> tighten filters and add reranking.
- Low recall on specific jargon -> add lexical/hybrid retrieval.
- Good offline metrics, poor real usage -> evaluate on real user queries, not only synthetic sets.
- Stale results -> refresh embeddings/index after content updates.
- Hallucinated final answers despite retrieval -> verify whether relevant context was actually retrieved and passed to generation.

Debug retrieval first, generation second.

---

## Suggested Hands-On Exercises

- Run similarity search on 100+ chunks and inspect top-5 for 20 real queries.
- Compare cosine vs dot product ranking (with/without normalization).
- Implement threshold-based fallback: if no strong match, return "insufficient context."
- Build a simple hybrid ranker (BM25 + vector score) and compare against vector-only.
- Track Recall@5 and MRR before and after adding reranking.

---

## What Comes Next

- Day 13 can build into full RAG system design: prompt grounding, citation strategy, and answer faithfulness checks.
- You will connect similarity retrieval quality directly to trustworthy AI output.

---

## References and Resources

- Intro material on information retrieval metrics and ranking quality.
- Embedding model docs for recommended similarity metric usage.
- Vector DB docs for ANN tuning and filtering patterns.
- Practical guides on hybrid retrieval and reranking pipelines.

Replace these placeholder bullets with your preferred concrete links and notebooks.
