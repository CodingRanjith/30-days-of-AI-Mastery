# Day 07 – Hugging Face & Open Models

**Theme:** Week 1 – Core Foundations (AI + LLM Internals)  
**Date:** 2026-04-07

---

## Learning Objectives

By the end of Day 7, you should be able to:

- Navigate the **Hugging Face Model Hub** and interpret model cards for practical selection.  
- Use `transformers` **pipeline** for quick inference and know when to switch to lower-level APIs.  
- Compare open models using **task fit**, **size**, **license**, and **hardware constraints**.  
- Explain the difference between **weights**, **tokenizer**, and **inference config** when running a model.  
- Build a minimal evaluation habit before adopting any open model in production.

---

## Why Hugging Face Matters

Hugging Face is the center of gravity for much of the open model ecosystem:

- A large **Model Hub** across NLP, vision, audio, and multimodal tasks.  
- Shared tooling (`transformers`, `datasets`, `tokenizers`, inference libraries).  
- Community-driven model cards, demos, and reproducible examples.

For AI engineers, it dramatically reduces time from idea to prototype.

---

## Core Concepts: What You Download and Run

When you "use a model," you are usually combining:

- **Model weights**: learned parameters.  
- **Tokenizer**: text-to-token and token-to-text rules.  
- **Config files**: architecture and generation defaults.  
- **Runtime backend**: PyTorch, ONNX, vLLM, TGI, llama.cpp, etc.

Important: swapping only the weights without matching tokenizer/config can cause incorrect behavior.

---

## Exploring the Model Hub Effectively

When browsing candidates, check these first:

1. **Task tag**: text-generation, embeddings, classification, ASR, vision, etc.  
2. **Model size**: parameter count and memory implications for your hardware.  
3. **License**: commercial use, redistribution, attribution, and restrictions.  
4. **Context length** and supported features (chat format, tool calling, multilingual support).  
5. **Model card quality**: training data notes, safety caveats, known limitations.

A strong model card is often a good signal of maintainability.

---

## Quick Start: `pipeline` vs Lower-Level APIs

### `pipeline` (fastest to start)

- Great for trying tasks quickly with minimal code.  
- Good for demos, exploratory comparisons, and baseline checks.

### Lower-level (`AutoModel*`, `AutoTokenizer`, generation args)

- Better control over batching, device placement, precision, and decoding params.  
- Needed for serious optimization, custom serving, and reproducible experiments.

Rule of thumb: prototype with `pipeline`, scale with lower-level control.

---

## Choosing an Open Model: Practical Checklist

- **Use case fit**: chat, coding, summarization, extraction, embeddings, reranking.  
- **Latency target**: real-time vs batch.  
- **Quality target**: acceptable error rate for your domain.  
- **Hardware budget**: VRAM/RAM constraints, quantization options.  
- **License/compliance**: legal fit for your company and geography.  
- **Ecosystem support**: community usage, examples, bug fixes, active maintenance.

Do not select based only on leaderboard rank. Validate on your own prompts and data.

---

## Common Pitfalls (And How to Avoid Them)

- **Ignoring license terms**: always read the exact model license before commercial use.  
- **No baseline eval**: run a small fixed test set before and after model changes.  
- **Tokenizer mismatch**: keep tokenizer + model paired.  
- **Prompt format mismatch**: many chat models need specific templates/tokens.  
- **Underestimating hardware needs**: test memory with realistic sequence lengths, not toy prompts.

---

## Suggested Hands-On Exercises

- Pick 2-3 candidate open chat models from the Hub and compare them on the same 10 prompts.  
- Record quality, latency, and approximate cost/hardware needs in a small table.  
- Re-run one model with different decoding params (temperature/top-p) and note behavior changes.  
- Read one model license fully and summarize what is allowed and what is restricted.

---

## What Comes Next

- **Day 8 – Data Preparation**: cleaning, normalization, and dataset formats for reliable AI pipelines.  
- Upcoming weeks connect model selection with retrieval, evaluation, and production deployment decisions.

---

## References and Resources

- Hugging Face **Model Hub** and model cards.  
- `transformers` and `datasets` official documentation.  
- Practical guides on quantization/inference backends (for running larger models efficiently).  
- Intro resources on evaluating open models with fixed prompt sets.

You can replace these placeholder bullets with your preferred links and notebooks.
