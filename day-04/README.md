# Day 04 – Embeddings Deep Dive

![Day 04 – Embeddings Deep Dive](day%204.png)

**Theme:** Week 1 – Core Foundations (AI + LLM Internals)  
**Date:** 2026-04-04

---

## Learning Objectives

By the end of Day 4, you should be able to:

- Explain what **embeddings** are and why models use **dense vector representations** of text.  
- Connect **embeddings** to the **Transformer** stack (token vectors, contextualization).  
- Describe **semantic similarity** in terms of distance or angle in embedding space.  
- Name common **similarity metrics** (e.g., cosine similarity) and when they matter.  
- Outline how embeddings power **search**, **clustering**, and **retrieval** at a high level (expanded in later days).

---

## Why Embeddings Matter

LLMs do not manipulate raw text directly inside the network. They map discrete tokens to **real-valued vectors** and compute on those vectors layer by layer.

An **embedding** is a way to represent something (a token, a sentence, a document) as a **point in a high-dimensional vector space**, such that **meaningful relationships** in the data show up as **geometric relationships** between vectors.

Embeddings are central to:

- **Inside the model**: token embeddings, positional information, and contextual states are all vector-based.  
- **Downstream systems**: semantic search, clustering, recommendation, and **RAG** (retrieval-augmented generation) all rely on comparing vectors.

---

## Vector Representations

A **vector** here is an ordered list of numbers, e.g. \( \mathbf{v} \in \mathbb{R}^d \) with dimension \(d\) (often hundreds or thousands).

- **Token embeddings**: each token ID maps to a row in an **embedding matrix**; the model learns these during training.  
- **Contextual embeddings**: after Transformer layers, each position’s vector depends on **the whole sequence**—same word can get different vectors in different contexts (“bank” river vs. money).

Properties you should internalize:

- **Dimension \(d\)**: larger \(d\) can represent richer structure, at the cost of more parameters and compute.  
- **Linear structure (often)**: analogies sometimes appear as vector differences (classic example: king − man + woman ≈ queen in some word embedding spaces). This is not guaranteed for all models but illustrates the idea that **directions in space can encode meaning**.

---

## Semantic Meaning in Embedding Space

**Semantics** is about meaning, not surface form. Embeddings aim to place **semantically similar** text **close together** in vector space, and **dissimilar** text **farther apart**.

What “close” means is defined by the **training objective** and data:

- Models trained on **next-token prediction** learn representations that predict well from context.  
- **Sentence / text embedding models** are often trained with contrastive or similarity objectives so that **paraphrases or related pairs** are pulled together and unrelated pairs are pushed apart.

Important caveats:

- Similarity in embedding space is **a model-dependent proxy** for semantic relatedness, not ground truth.  
- Short texts can be ambiguous; **context** and **task** still matter.  
- Later in the roadmap you will choose **specialized embedding models** for retrieval (Day 10) and combine them with **vector databases** (Day 11).

---

## Similarity Search Basics

To compare two embeddings \( \mathbf{a} \) and \( \mathbf{b} \), common choices are:

### Cosine similarity

Measures the **cosine of the angle** between two vectors (orientation, not magnitude):

\[
\text{cosine}(\mathbf{a}, \mathbf{b}) = \frac{\mathbf{a} \cdot \mathbf{b}}{\|\mathbf{a}\| \, \|\mathbf{b}\|}
\]

- Range typically **−1 to 1** for real vectors (often **0 to 1** if vectors are non-negative or normalized).  
- **1** means same direction; **0** means orthogonal; **−1** means opposite direction.  
- Very common in NLP because it focuses on **direction** (semantic “topic” in many setups), which is often more stable than raw dot product when vector lengths vary.

### Dot product

\[
\mathbf{a} \cdot \mathbf{b} = \sum_i a_i b_i
\]

- If vectors are **normalized to unit length**, dot product equals **cosine similarity**.  
- With unnormalized vectors, magnitude affects the score (sometimes desired, sometimes not).

### Euclidean distance

\[
\|\mathbf{a} - \mathbf{b}\|_2
\]

- **Smaller** distance means **closer** in space.  
- Relates to dot product when vectors are normalized; in practice, cosine is often preferred for text similarity.

For **search**: given a **query embedding**, you rank a **corpus** of precomputed embeddings by similarity (or distance). At small scale you can brute-force compare to every item; at large scale you use **approximate nearest neighbor (ANN)** indexes (covered on Day 12).

---

## Embeddings Inside LLMs vs Standalone Embedding Models

| Idea | Role |
|------|------|
| **Token / hidden states inside an LLM** | Intermediate representations for generation; highly contextual. |
| **Dedicated embedding APIs / models** | Map text (or images) to a single vector per chunk for **search**, **classification**, or **retrieval**. |

You will reuse these ideas when you build **RAG**: embed queries and documents, then retrieve the most similar chunks before prompting the LLM.

---

## What Comes Next

- **Day 5 – Tokenization**: how text becomes tokens (and how that interacts with embeddings and cost).  
- **Later weeks**: **embedding model choice**, **chunking**, **vector DBs**, **hybrid retrieval**, and **RAG** pipelines.

---

## References and Resources

- **Concepts**: introductory articles on **word embeddings**, **sentence embeddings**, and **semantic search**.  
- **Math**: short primers on **cosine similarity**, **dot product**, and **Euclidean distance** in the context of ML.  
- **Tools (exploration)**: small demos that embed two sentences and show similarity scores (many model providers and open-source libraries offer this).  
- **Bridge to RAG**: a high-level blog or video on **“embeddings for retrieval”** or **vector search** basics.

You can replace these placeholder bullets with your favorite concrete links and notebooks.
