# Day 09 - Chunking Strategies

![Day 09 - Chunking Strategies](day%209.png)

**Theme:** Week 2 - Retrieval and Context Engineering  
**Date:** 2026-04-09

---

## Learning Objectives

By the end of Day 9, you should be able to:

- Explain why chunking quality directly impacts retrieval quality in RAG systems.  
- Compare fixed-size, semantic, and structure-aware chunking approaches.  
- Select chunk size and overlap based on content type and query patterns.  
- Identify common chunking failure modes and how to debug them quickly.  
- Build a practical chunking evaluation loop before shipping to production.

---

## Why Chunking Matters

In retrieval-augmented generation (RAG), chunking is the bridge between raw documents and useful context.

If chunks are too large, retrieval returns noisy context.  
If chunks are too small, retrieval loses meaning and continuity.

Good chunking improves:

- **Retrieval precision** (fewer irrelevant passages).  
- **Recall** (higher chance the needed fact is retrieved).  
- **Answer grounding** (responses cite coherent source spans).  
- **Latency and cost** (fewer wasted tokens in prompts).

---

## What Is a "Chunk"?

A chunk is a segment of source content stored in the vector index (or hybrid index), usually with:

- **Text content** (the passage itself).  
- **Metadata** (document ID, section title, page, timestamp, tags).  
- **Optional hierarchy info** (chapter -> section -> paragraph lineage).

Think of chunking as a packaging decision for knowledge retrieval.

---

## Core Chunking Strategies

### 1) Fixed-size chunking

Split text by character or token count (for example, 500 tokens with 50-token overlap).

- **Pros:** simple, fast, predictable.  
- **Cons:** can split ideas mid-sentence or mid-table.

Best when you need a baseline quickly.

### 2) Semantic chunking

Split where meaning changes (topic shifts, discourse boundaries, embedding similarity drops).

- **Pros:** better conceptual coherence.  
- **Cons:** more complex implementation and tuning.

Best for long narrative docs, reports, and knowledge articles.

### 3) Structure-aware chunking

Use document structure: headings, sections, bullet groups, code blocks, tables, transcripts.

- **Pros:** preserves natural units and improves traceability.  
- **Cons:** depends on good source formatting/parsing.

Best for manuals, policies, docs sites, and technical specs.

---

## Choosing Chunk Size and Overlap

There is no universal best value. Use content and query style to decide:

- **FAQ / short factual docs:** smaller chunks (150-350 tokens), low overlap.  
- **Technical docs / API guides:** medium chunks (300-700 tokens), moderate overlap.  
- **Narrative or legal text:** larger coherent chunks (500-1000 tokens), careful overlap.

Overlap helps preserve continuity, but too much overlap creates near-duplicates and increases index size.

Practical starting point:

- Chunk size: **400-600 tokens**  
- Overlap: **10-20%**

Then tune with real queries.

---

## Metadata Is Part of Chunking

Retrieval quality is not only text splitting. Metadata filters often decide final relevance.

Useful metadata fields:

- `source` (file/doc ID)  
- `section_title`  
- `created_at` or `version`  
- `language`  
- `topic` or custom tags

Good metadata enables scoped retrieval ("latest policy", "billing section only", "English docs only").

---

## Common Failure Modes

- **Boundary breaks:** key fact split across two chunks with no overlap.  
- **Context dilution:** huge chunks include irrelevant paragraphs.  
- **Header-only chunks:** title retrieved without enough body text.  
- **Duplicate inflation:** excessive overlap fills top-k with near-identical results.  
- **Parser mismatch:** markdown/PDF parsing errors create noisy or broken chunks.

When answers are "almost right," chunking is often the root cause.

---

## Evaluation Workflow (Minimal but Effective)

1. Build a fixed test set of 20-50 realistic user queries.  
2. Define expected source passages for each query (or at least expected docs/sections).  
3. Run retrieval-only evaluation first (`top_k`, MRR/Recall@k, manual relevance checks).  
4. Compare chunking variants while keeping embedding model and retriever fixed.  
5. Promote only changes that improve both relevance and downstream answer quality.

Do retrieval evaluation before prompt engineering changes; otherwise you may hide indexing issues.

---

## Suggested Hands-On Exercises

- Implement a baseline fixed-token chunker and inspect 30 random chunks manually.  
- Create a second variant with heading-aware splitting and compare retrieval outcomes.  
- Measure how overlap at 0%, 10%, and 20% affects top-k diversity.  
- Add metadata filters and test targeted queries ("from release notes only", "from security docs only").

---

## What Comes Next

- **Day 10 - Embeddings and Vector Stores**: mapping chunks into searchable vector space.  
- You will connect chunking decisions to embedding model choice, index design, and reranking.

---

## References and Resources

- Vendor docs for tokenization and context windows in your chosen model stack.  
- Retrieval evaluation frameworks (lightweight scripts are enough to start).  
- Parsing tools for markdown, HTML, and PDFs to preserve structure before chunking.  
- Practical RAG guides comparing chunking and retrieval strategies.

Replace these placeholder bullets with your preferred tools and links.
