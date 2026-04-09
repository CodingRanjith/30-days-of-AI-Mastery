# Day 13 - Retrievers

**Theme:** Week 2 - Retrieval, Search, and Knowledge Grounding  
**Date:** 2026-04-13

---

## Learning Objectives

By the end of Day 13, you should be able to:

- Explain what a **retriever** does in search and RAG systems.  
- Compare **sparse retrieval** (BM25), **dense retrieval** (embeddings), and **hybrid retrieval**.  
- Identify when lexical matching beats semantic matching, and vice versa.  
- Design a practical retrieval pipeline with filtering, ranking, and optional reranking.  
- Evaluate retriever quality using metrics like Recall@k, MRR, and hit quality checks.

---

## Why Retrievers Matter

In knowledge-grounded AI, generation quality is capped by retrieval quality.

If your retriever fetches irrelevant or incomplete context:

- The LLM produces weak or hallucinated answers.  
- Groundedness drops, even with a strong model.  
- Trust in the system falls quickly for users.

A good retriever is often the highest-leverage component in a RAG stack.

---

## What Is a Retriever?

A **retriever** is the component that finds candidate documents/chunks relevant to a query.

Typical placement in a RAG pipeline:

1. User asks a question.  
2. Retriever selects top-k context chunks.  
3. Prompt composer injects those chunks into the LLM context.  
4. LLM generates a grounded answer.

Think of retrieval as **evidence selection** before generation.

---

## Sparse Retrieval (BM25 / Lexical Search)

Sparse retrievers rely on exact or near-exact term overlap between query and documents.

Common characteristics:

- Strong when exact wording matters: legal terms, IDs, product codes, error strings.  
- Interpretable scoring (term frequency + inverse document frequency style signals).  
- Fast and robust for keyword-heavy enterprise search.

Limitations:

- Weak on paraphrases and semantic intent when wording differs.  
- Struggles when user language does not match document vocabulary.

---

## Dense Retrieval (Embedding-Based)

Dense retrievers convert query and documents into vectors and rank by similarity.

Common strengths:

- Better semantic matching for paraphrases and natural-language queries.  
- Handles concept similarity even without exact token overlap.  
- Works well for FAQ/chat assistants and broad knowledge search.

Limitations:

- Can miss exact-term requirements (codes, strict legal clauses).  
- Quality depends heavily on embedding model, chunking, and index settings.

---

## Hybrid Retrieval (Best of Both)

Hybrid retrieval combines lexical and semantic signals, typically by:

- Running sparse and dense retrieval in parallel.  
- Merging results with weighted scoring or rank fusion (e.g., Reciprocal Rank Fusion).  
- Returning top-k after fusion and filtering.

Why teams choose hybrid:

- Better robustness across varied query types.  
- Preserves exact-match precision while capturing semantic intent.

In real systems, hybrid is often the safest default starting point.

---

## Practical Retriever Pipeline

A production-friendly retrieval flow often looks like:

1. **Query preprocessing** (normalize, optional rewrite/expansion).  
2. **Dual retrieval** (sparse + dense, or one strategy initially).  
3. **Metadata filtering** (tenant, date, source, permissions).  
4. **Score fusion / ranking**.  
5. **Optional reranking** with a cross-encoder for top-N candidates.  
6. **Context assembly** for LLM prompt (with source citations if needed).

Small improvements at each stage often outperform changing only the LLM.

---

## When to Use Which Retriever

- Prefer **sparse** when:
  - Queries include exact identifiers, regulations, clauses, or code snippets.  
  - Precision on exact terms is critical.

- Prefer **dense** when:
  - Users ask natural questions with varied phrasing.  
  - Semantic similarity matters more than exact wording.

- Prefer **hybrid** when:
  - Query patterns are mixed and unpredictable.  
  - You need stable quality across domains.

---

## Evaluation: How to Know If Retrieval Is Good

Use both offline metrics and qualitative checks:

- **Recall@k**: did relevant chunks appear in top-k?  
- **MRR**: how early did the first relevant result appear?  
- **Precision@k / nDCG** (optional): ranking quality beyond first hit.  
- **Human spot checks**: are results actually useful as LLM context?

Also test edge cases:

- Acronyms, typos, multilingual phrasing, and very short queries.  
- Time-sensitive documents with metadata constraints.

---

## Common Failure Modes

- Over-chunked or under-chunked documents reduce retriever quality.  
- Missing metadata filters returns irrelevant or unauthorized results.  
- Dense retriever only setup fails exact-ID queries.  
- Sparse-only setup misses semantic paraphrases.  
- Top-k too small hides needed context; top-k too large adds noise.

Treat retriever tuning as an iterative engineering loop, not a one-time config task.

---

## Suggested Hands-On Exercises

- Run the same 20 queries through sparse-only, dense-only, and hybrid retrieval.  
- Measure Recall@5 and compare qualitative relevance for each method.  
- Add a simple reranker for top-20 candidates and evaluate ranking improvements.  
- Create a short "query taxonomy" (exact-match vs semantic vs mixed) and map best strategy per type.

---

## What Comes Next

- **Day 14 - RAG Fundamentals**: end-to-end architecture from ingestion to grounded generation.  
- Later topics build on this with orchestration, evaluation loops, and production hardening.

---

## References and Resources

- Intro resources on **BM25** and lexical ranking.  
- Guides on embedding-based retrieval and ANN/vector indexing.  
- Hybrid retrieval and rank-fusion explainers (including RRF).  
- Practical RAG evaluation writeups focused on retrieval quality first.

You can replace these placeholder bullets with concrete tools, libraries, and links you use.
