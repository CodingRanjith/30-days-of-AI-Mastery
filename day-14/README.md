# Day 14 - RAG Fundamentals

![Day 14 - RAG Fundamentals](day%2014.png)

**Theme:** Week 3 - Retrieval-Augmented Systems  
**Date:** 2026-04-14

---

## Learning Objectives

By the end of Day 14, you should be able to:

- Explain what Retrieval-Augmented Generation (RAG) is and when to use it.  
- Describe the full RAG pipeline from query intake to grounded answer generation.  
- Separate responsibilities of chunking, embedding, indexing, retrieval, and generation.  
- Identify common RAG failure modes and apply practical mitigations.  
- Build a minimum viable RAG system and evaluate answer quality with citations.

---

## Why RAG Matters

Base LLMs are powerful, but they have limitations:

- They may hallucinate facts.  
- Their knowledge can be stale.  
- They cannot reliably access private company data by default.

RAG solves this by attaching an external knowledge system:

- Retrieve relevant context from your documents at runtime.  
- Feed that context into the model prompt.  
- Generate answers grounded in retrieved evidence.

RAG is often the fastest path to useful, domain-specific AI without expensive model fine-tuning.

---

## RAG in One Sentence

RAG = **search first, generate second**.

Instead of asking the model to answer from memory alone, you provide task-relevant context from a trusted knowledge base for each query.

---

## Core RAG Architecture

A practical RAG system usually has these layers:

1. **Ingestion pipeline**  
   - Parse source docs (PDFs, docs, wikis, tickets, code).  
   - Clean and normalize text.  
   - Chunk into searchable passages.  
   - Compute embeddings and store in vector index.

2. **Online query pipeline**  
   - Receive user query.  
   - Retrieve top-k relevant chunks (vector, keyword, or hybrid).  
   - Optionally rerank results.  
   - Build grounded prompt with instructions + retrieved context.  
   - Generate answer and return citations.

3. **Evaluation and monitoring**  
   - Measure retrieval quality and answer faithfulness.  
   - Track latency, token cost, and failure patterns.  
   - Iterate on chunking, retrieval settings, and prompting.

---

## Building Blocks (And What Each One Controls)

### 1) Chunking

Chunking determines what retrieval units the system can return.

- Too large: noisy, expensive context windows.  
- Too small: missing necessary context and coherence.

Typical starting point:

- 300-800 tokens per chunk  
- 10-20% overlap for continuity  
- Keep metadata (`source`, `title`, `section`, `timestamp`)

### 2) Embeddings

Embeddings convert text into vectors for semantic search.

- Better embedding model -> better recall for meaning-based queries.  
- Domain mismatch hurts retrieval quality.

### 3) Vector Index

The index stores embeddings for fast nearest-neighbor search.

- Supports similarity search and metadata filters.  
- Choice of database impacts latency and scalability, not just quality.

### 4) Retriever

Retriever selects candidate chunks.

- `top_k` is critical: too low misses context, too high adds noise.  
- Hybrid retrieval (keyword + vector) often improves robustness.

### 5) Generator Prompt

Prompt should instruct model to:

- Answer only from provided context.  
- Say "I don't know" when evidence is insufficient.  
- Cite sources/sections used.

Clear constraints reduce hallucination significantly.

---

## Retrieval Strategies You Should Know

- **Dense retrieval (vector):** good for semantic similarity and paraphrases.  
- **Sparse retrieval (keyword/BM25):** good for exact terms, IDs, version strings.  
- **Hybrid retrieval:** combines dense + sparse; often strongest default in production.  
- **Reranking:** reorders candidates with cross-encoders or LLM-based scoring for better precision.

Start simple: dense or hybrid retrieval + basic reranking if quality is weak.

---

## Common RAG Failure Modes

1. **Bad chunks**  
   - Split logic destroys meaning boundaries.

2. **Wrong retrieval**  
   - Top results are related but not directly answering the query.

3. **Prompt leakage/noise**  
   - Too much irrelevant context causes confused outputs.

4. **No citation discipline**  
   - Model gives polished answers without verifiable grounding.

5. **Stale index**  
   - New or updated documents are not re-embedded.

Mitigation approach:

- Fix retrieval before changing the model.  
- Improve chunking and metadata filters.  
- Add reranking and stricter prompt constraints.  
- Require citations and fallback behavior.

---

## Minimum Viable RAG Checklist

Before calling your RAG system "ready," ensure:

- Ingestion pipeline is reproducible.  
- Metadata fields support filtering and citation.  
- Retrieval baseline is measured on a small gold query set.  
- Prompt enforces grounded answers and abstention behavior.  
- Output includes source references for every factual claim.  
- Basic observability exists (query logs, hit rates, latency, error cases).

---

## Suggested Hands-On Exercises

- Build a mini RAG app on 20-50 documents from one domain.  
- Compare vector-only vs hybrid retrieval on the same query set.  
- Tune chunk size and top-k; document quality/latency trade-offs.  
- Add citation formatting and test whether claims can be traced back to chunks.  
- Create 10 "unanswerable" queries and verify abstention behavior.

---

## What Comes Next

- **Day 15 - RAG Evaluation and Improvement:** retrieval metrics, faithfulness checks, and iterative tuning loops.  
- You will move from "it works" to "it is reliable enough for production decisions."

---

## References and Resources

- Intro guides for RAG architecture and retrieval design patterns.  
- Documentation for your chosen vector store and embedding model.  
- Retrieval evaluation resources (precision/recall and ranking metrics).  
- Practical papers/posts on hallucination reduction and grounded generation.

You can replace these placeholder bullets with your preferred links, notebooks, and implementation stack docs.
