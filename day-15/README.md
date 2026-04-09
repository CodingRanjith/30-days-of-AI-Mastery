# Day 15 - Advanced RAG

**Theme:** Week 3 - Retrieval Systems and Production Patterns  
**Date:** 2026-04-15

---

## Learning Objectives

By the end of Day 15, you should be able to:

- Explain why naive RAG pipelines fail on complex user queries.  
- Design a multi-stage retrieval pipeline (retrieve -> rerank -> generate).  
- Apply hybrid retrieval (vector + keyword) to improve relevance and robustness.  
- Use query rewriting and decomposition for ambiguous or multi-hop questions.  
- Evaluate advanced RAG with retrieval metrics and grounded answer checks.

---

## Why "Basic RAG" Stops Working

A simple RAG pipeline often looks like:

1. Embed user query  
2. Retrieve top-k chunks  
3. Pass chunks to LLM  
4. Generate answer

This works for straightforward factual lookups, but fails when:

- User queries are vague or underspecified.  
- Relevant evidence is spread across multiple documents.  
- Keywords matter more than semantic similarity (IDs, codes, exact terms).  
- Retrieved chunks are relevant-but-not-best, and generation hallucinates gaps.

Advanced RAG adds control points between retrieval and generation.

---

## Multi-Stage Retrieval Architecture

A practical production flow:

1. **Query understanding** (optional rewrite/decompose).  
2. **Candidate retrieval** (high recall, larger top-k).  
3. **Reranking** (high precision, choose best passages).  
4. **Context assembly** (dedupe, order, trim to budget).  
5. **Grounded generation** (citation-aware answer prompt).

This "funnel" approach balances recall and precision.

---

## Hybrid Retrieval: Dense + Sparse

Relying only on embeddings misses exact-match signals. Relying only on keyword search misses semantic similarity.

Hybrid retrieval combines:

- **Dense search** (vector similarity for semantic intent).  
- **Sparse search** (BM25/keyword for exact terms and rare tokens).

Why this helps:

- Better handling of product IDs, error codes, legal references, and names.  
- Improved robustness for short or noisy queries.  
- More stable retrieval across domains with specialized vocabulary.

Common implementation patterns:

- Score fusion (weighted dense + sparse scores).  
- Retrieve separately, then merge and deduplicate.  
- Route by query type (for example, exact-code queries favor sparse).

---

## Reranking for Precision

Initial retrieval is optimized for speed/recall. Reranking improves final context quality.

Reranker input:

- Query + candidate passages (for example top 30-100).  
- Output relevance score per passage.

Benefits:

- Removes weakly related chunks from final context.  
- Increases grounding quality with fewer tokens.  
- Improves answer correctness on "close call" queries.

Rule of thumb:

- Retrieve broader (`k` high), rerank tighter (`k` low), then generate.

---

## Query Rewriting and Decomposition

Many failures start with a weak query representation.

### Query rewriting

Normalize user wording into retrieval-friendly form:

- Expand acronyms.  
- Add missing domain terms.  
- Clarify implied entity or timeframe.

### Query decomposition

For multi-hop questions, split into sub-questions:

- Retrieve evidence per sub-question.  
- Merge evidence.  
- Generate a final synthesis.

This is especially useful for comparison, causality, and timeline questions.

---

## Context Assembly Matters

Even great retrieval can fail if context is assembled poorly.

Best practices:

- Deduplicate near-identical chunks.  
- Preserve source metadata and section titles.  
- Order chunks by relevance and logical flow.  
- Enforce a strict token budget with quality-first truncation.  
- Pass citations/sources explicitly in prompt format.

Context assembly is where many hidden quality gains live.

---

## Grounded Generation Patterns

Prompt the model to:

- Answer only from provided sources.  
- Cite source IDs/sections for each key claim.  
- State uncertainty when evidence is missing.

Useful guardrails:

- "If no supporting evidence exists, say you cannot determine."  
- "Do not use prior knowledge beyond provided context."  
- "Return claim -> citation mapping."

This reduces confident hallucinations.

---

## Evaluation for Advanced RAG

Do not evaluate only final answer fluency. Track retrieval and grounding separately.

Minimum evaluation stack:

- **Retrieval metrics:** Recall@k, MRR, nDCG.  
- **Reranking impact:** delta in precision/Recall@k before vs after reranker.  
- **Grounding checks:** percentage of claims with valid citations.  
- **Task success:** human or rubric score on correctness and completeness.

Create a benchmark set with:

- Simple factual queries  
- Exact-match queries (IDs/codes)  
- Multi-hop reasoning queries  
- Ambiguous queries needing clarification

---

## Common Failure Modes in Advanced RAG

- **Over-retrieval noise:** high `k` without reranking floods context.  
- **Fusion imbalance:** dense/sparse weights tuned poorly for domain.  
- **Rewrite drift:** rewritten query changes user intent.  
- **Citation theater:** model cites irrelevant chunk IDs.  
- **Evaluation blind spots:** only checking final answer style, not evidence quality.

Debug in stages: retrieve -> rerank -> assemble -> generate.

---

## Suggested Hands-On Exercises

- Build a hybrid retriever and compare it with dense-only on 30 domain queries.  
- Add a reranker step and measure precision gain at final context `k`.  
- Implement a simple query rewrite step and evaluate when it helps vs hurts.  
- Create a multi-hop mini benchmark and test decomposition strategy.

---

## What Comes Next

- **Day 16 - RAG Evaluation and Guardrails:** deeper evaluation design, safety checks, and reliability gates.  
- You will convert advanced retrieval quality into measurable production standards.

---

## References and Resources

- Hybrid search and reranking docs from your vector DB/search stack.  
- Retrieval evaluation guides (MRR, nDCG, Recall@k) with practical examples.  
- Papers and engineering blogs on multi-stage retrieval pipelines.  
- Grounded generation/citation prompting best practices for enterprise RAG.

Replace these placeholders with your preferred links, libraries, and internal standards.
