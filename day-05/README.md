# Day 05 – Tokenization

![Day 05 – Tokenization](Day%2005.png)

**Theme:** Week 1 – Core Foundations (AI + LLM Internals)  
**Date:** 2026-04-05

---

## Learning Objectives

By the end of Day 5, you should be able to:

- Explain why LLMs operate on **tokens** instead of raw characters or whole words.  
- Compare **BPE**, **WordPiece**, and **SentencePiece** at a high level (what problem each solves).  
- Describe how **subword** tokenization balances **vocabulary size**, **unknown words**, and **efficiency**.  
- Relate **token counts** to **context limits**, **latency**, and **API cost**.  
- Estimate and reason about token usage for typical prompts (rough order-of-magnitude thinking).

---

## Why Tokenization Matters

**Tokenization** is the first real step in an LLM pipeline: it turns raw text into a sequence of **token IDs** the model can embed and process.

Nothing in the neural network “sees” your string directly—it sees **integers** pointing into a **vocabulary**. How text is split into those integers affects:

- **Length** of the sequence (and whether it fits in the **context window**).  
- **Cost** (many APIs charge per token).  
- **Behavior** (punctuation, code, numbers, and multilingual text may split in unintuitive ways).  
- **Robustness** to rare words, typos, and out-of-vocabulary strings.

Tokenization is not a cosmetic detail; it is part of the **interface** between your application and the model.

---

## Tokens vs Words vs Characters

| Unit | Pros | Cons |
|------|------|------|
| **Characters** | Huge vocabulary is tiny; no unknown characters | Sequences are very long; harder to capture word-level meaning efficiently. |
| **Words** (whitespace / dictionary) | Human-readable units | Huge vocabularies; **OOV** (unknown words) for names, typos, morphology; many languages don’t split cleanly on spaces. |
| **Subwords** (common today) | Balances vocab size and flexibility; rare words built from pieces | Splits can look odd; token↔word alignment is not 1:1. |

Modern LLMs overwhelmingly use **subword** tokenizers so they can represent both frequent words and rare strings without an exploding vocabulary.

---

## Subword Tokenization: BPE, WordPiece, SentencePiece

These are all **data-driven** ways to learn a vocabulary of subword units from a corpus. They differ in details, but the shared goal is: **compress text into a small set of frequent pieces** while still being able to encode anything.

### Byte-Pair Encoding (BPE)

- Starts from characters (or bytes) and **iteratively merges** the most frequent pairs of symbols into new tokens.  
- Produces a fixed-size vocabulary of merges; encoding is **greedy** longest-match style given that vocabulary.  
- Used (with variants) in many GPT-style setups.

### WordPiece (e.g., BERT family)

- Similar merge idea, often framed around **maximizing language-model likelihood** of the training data given the vocabulary.  
- Uses a special **`##`** (or similar) marker for continuations inside words in some implementations.

### SentencePiece

- Treats the input as a **raw stream** (often Unicode) and learns subwords **without** requiring pre-tokenization by spaces—useful for **multilingual** and **no-space** languages.  
- Can wrap **BPE** or **unigram** language models under one library API.

You do not need to memorize every algorithmic step for interviews on Day 5; you **do** need to know that **different models ship with different tokenizers**, and **you cannot assume** two models tokenize the same prompt the same way.

---

## Intuition: Why “Strange” Splits Happen

- **Numbers and punctuation** often become several tokens (e.g., long numbers split).  
- **Code** has many symbols; each may be its own token or merge depending on frequency.  
- **Proper nouns** might be one token or many subwords.  
- **Leading/trailing spaces** in some tokenizers become special tokens—important for formatting-sensitive tasks.

When debugging prompts, it helps to **inspect tokens** with the same tokenizer the target model uses (provider playgrounds often show this).

---

## Context Windows, Token Limits, and Cost

- Providers publish **maximum context** in **tokens** (input + output rules vary by product).  
- If your prompt + retrieved documents + instructions exceed the limit, you must **truncate**, **summarize**, or **chunk** (RAG days go deeper).  
- **Billing** is often per **1K tokens** or per **million tokens**; long prompts and long completions add up quickly.

Rough habits that help:

- **Measure** token counts during development (tokenizer for your model, or API usage metrics).  
- **Design prompts** to put the most important constraints and context **early** if truncation is possible.  
- **Reserve** output budget when you need long answers (max tokens / response length).

---

## Practical Implications for Applications

- **Prompt engineering**: shorter is not always better, but **redundant** text wastes tokens without helping.  
- **RAG**: chunk sizes and overlap are often expressed in **tokens** or characters—token-aware chunking aligns better with model limits.  
- **Multilingual**: byte-level or SentencePiece-style tokenizers can handle many scripts; still **test** your languages.  
- **Determinism**: tokenizer + model are coupled; swapping models often means **re-validating** prompts and limits.

---


## What Comes Next

- **Day 6 – Model Ecosystem**: mapping **OpenAI**, **LLaMA**, **Gemini**, and trade-offs (open vs closed, pricing, licensing).  
- **Later**: retrieval, embeddings at scale, and full **RAG** pipelines where tokenization, chunking, and context limits all interact.

---

## References and Resources

- **Surveys / explainers**: articles or videos on **subword tokenization**, **BPE**, and **SentencePiece**.  
- **Docs**: your model provider’s page on **tokenizer behavior**, **context limits**, and **pricing per token**.  
- **Tools**: official tokenizer libraries (`tiktoken` for OpenAI models, **Hugging Face tokenizers**, etc.) and playground “token view” UIs.

You can replace these placeholder bullets with specific links and notebooks you prefer.
