# Day 11 - Vector Databases

**Theme:** Week 2 - Retrieval, Search, and Knowledge Grounding  
**Date:** 2026-04-11

---

## Learning Objectives

By the end of Day 11, you should be able to:

- Explain why vector databases are used for semantic retrieval at scale.
- Understand approximate nearest neighbor (ANN) search and its speed/accuracy trade-off.
- Compare common index types (HNSW, IVF, PQ) at a practical level.
- Design a retrieval stack with metadata filtering, hybrid search, and reranking.
- Operate a vector index safely with updates, reindexing, and monitoring.

---

## Why Vector Databases Exist

For small datasets, brute-force similarity search can work.  
For production-scale corpora, exact search becomes too slow and expensive.

Vector databases solve this by:

- Storing embeddings efficiently.
- Supporting fast nearest-neighbor retrieval.
- Combining vector search with metadata filtering.
- Providing operational tooling for upserts, deletes, and index maintenance.

They are a core building block in modern RAG systems.

---

## Exact Search vs ANN Search

### Exact nearest neighbor

- Compares query vector against every vector in the dataset.
- High accuracy, but latency grows quickly with dataset size.
- Useful for small collections or offline evaluation.

### Approximate nearest neighbor (ANN)

- Uses specialized index structures to avoid scanning all vectors.
- Much faster and cheaper at scale.
- May miss some true nearest neighbors (small recall trade-off).

In real systems, ANN is usually the default, tuned to hit target recall and latency.

---

## Common ANN Index Families

### HNSW (Hierarchical Navigable Small World)

- Graph-based index with strong practical recall/latency balance.
- Good default for many retrieval workloads.
- Main knobs often include construction and search effort parameters.

### IVF (Inverted File Index)

- Partitions vector space into clusters; searches relevant clusters only.
- Efficient for large datasets.
- Requires tuning number of clusters and probes.

### PQ (Product Quantization)

- Compresses vectors to reduce memory/storage footprint.
- Faster and cheaper at scale, with some precision loss.
- Often combined with IVF in large deployments.

There is no single best index type. Choose based on dataset size, latency budget, memory limits, and required recall.

---

## Vector Database Architecture in Practice

A practical retrieval stack commonly includes:

1. Embedding generation service.
2. Vector store/index for ANN search.
3. Metadata store/filter layer (source, tenant, ACL, freshness).
4. Optional lexical engine for hybrid search.
5. Reranker for improving top-k quality before answer generation.

Query flow:

- User query -> embed -> ANN retrieve top-k -> apply filters/hybrid merge -> rerank -> pass best context to LLM.

---

## Metadata Filtering Is Not Optional

Raw semantic similarity alone is rarely enough.

Use metadata filters for:

- Tenant/workspace isolation in multi-tenant apps.
- Security and document-level access control.
- Time bounds (freshness windows).
- Source or content-type narrowing (docs, tickets, runbooks, code).

Strong filtering improves precision and reduces irrelevant retrieval.

---

## Hybrid Retrieval and Reranking

### Hybrid retrieval

Combine:

- Lexical search (exact terms, IDs, rare words).
- Semantic search (intent and paraphrase matching).

This usually outperforms either method alone.

### Reranking

A reranker (often cross-encoder style) rescoring top candidates can significantly improve final relevance.

Typical pattern:

- Retrieve top 20-100 quickly.
- Rerank top candidates with a stronger model.
- Keep best passages for answer generation.

---

## Operational Concerns You Must Handle

- Upserts and deletes: keep index synchronized with source content.
- Re-embedding strategy: reprocess when content changes or model upgrades.
- Backfills: safe bulk indexing pipelines for large imports.
- Versioning: track embedding model/index version for reproducibility.
- Monitoring: latency, recall proxies, empty-result rate, and drift indicators.
- Disaster readiness: backup/restore strategy for index + metadata.

A retrieval system is not just a model choice; it is an ongoing data system.

---

## Tuning Knobs and Trade-Offs

When tuning ANN, you usually balance:

- Recall vs latency.
- Memory footprint vs precision.
- Ingestion speed vs query performance.

Useful habits:

- Keep a fixed evaluation set.
- Tune one knob at a time.
- Record metrics before and after each change.
- Separate offline quality evaluation from online latency/load tests.

---

## Common Pitfalls (And Fixes)

- Using default index settings forever -> run periodic tuning with real traffic patterns.
- Ignoring ACL metadata in retrieval -> enforce authorization-aware filtering.
- Re-embedding without version tracking -> store model/version in metadata.
- Over-compressing vectors too early -> benchmark quality loss before adopting heavy quantization.
- No reranking stage -> add reranker when top-k relevance is noisy.
- No evaluation harness -> maintain query-relevance test set with Recall@k and MRR/NDCG-style metrics.

---

## Suggested Hands-On Exercises

- Index a medium corpus (1k-10k chunks) and compare exact vs ANN latency/recall.
- Test two ANN configurations and document recall@10 and p95 latency.
- Add metadata filters (e.g., by source/date/team) and measure precision changes.
- Build a simple hybrid retriever (BM25 + vector merge) and compare to vector-only baseline.
- Add reranking for top 30 candidates and record final answer-quality improvement.

---

## What Comes Next

- Day 12 can naturally move into end-to-end RAG architecture, prompt grounding, and retrieval evaluation loops.
- You will connect retrieval engineering decisions directly to answer faithfulness and user trust.

---

## References and Resources

- Intro materials on ANN fundamentals and vector index design.
- Vendor or open-source vector DB docs for your chosen stack.
- Hybrid search and reranking implementation guides.
- Practical retrieval evaluation resources (Recall@k, MRR, NDCG, hit rate).

Replace these placeholder bullets with your preferred links, notebooks, and repos.
