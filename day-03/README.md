# Day 03 – Transformers Deep Dive

![Day 03 – Transformers Architecture](Day%2003.png)

**Theme:** Week 1 – Core Foundations (AI + LLM Internals)  
**Date:** 2026-04-03

---

## Learning Objectives

By the end of Day 3, you should be able to:

- Explain the **core Transformer architecture** in plain language.  
- Describe how **self-attention** works and why it replaced RNNs for many tasks.  
- Understand **multi-head attention**, **positional encodings**, and **residual + layer norm** blocks.  
- Compare **encoder–decoder** vs **decoder-only** Transformers (like most modern LLMs).  
- Read a simple attention diagram or pseudocode and connect it to the high-level behavior of LLMs.

---

## Why Transformers Matter

Transformers are the **backbone architecture** behind almost all modern LLMs and many state-of-the-art models in vision, audio, and multimodal AI.

Before Transformers, most sequence models used:

- **RNNs / LSTMs / GRUs**: processed tokens one-by-one, left to right.  
- These struggled with **long-range dependencies**, parallelization, and training stability on very large datasets.

Transformers introduced:

- **Self-attention**: every token can directly look at **all other tokens** in the sequence.  
- **Parallel computation** over all positions (no strict left-to-right recurrence during training).  
- Much better **scaling** to large data and large model sizes.

Result: when combined with large datasets and compute, Transformers unlocked the era of **large language models**.

---

## High-Level Transformer Architecture

At a high level, a Transformer is a **stack of layers**, and each layer contains:

1. **Multi-Head Self-Attention** (or cross-attention in encoder–decoder).  
2. **Feed-Forward Network (FFN)** applied position-wise.  
3. **Residual connections** and **Layer Normalization** around these sub-layers.

You can picture one block as:

- Input embeddings → Self-Attention → Add & Layer Norm → FFN → Add & Layer Norm.

Stack many of these blocks on top of each other:

- Lower layers capture more **local / syntactic** patterns.  
- Higher layers capture more **semantic / abstract** relationships.

---

## Self-Attention: Intuition First

Self-attention answers the question:

> “For this token, **which other tokens in the sequence are important** to look at, and by how much?”

For each token, we:

- Compute a **weighted average** of all token representations in the sequence.  
- The weights are called **attention weights** and are learned by the model.

This means:

- A token like `"it"` can look back at `"the cat"` or `"the company"` and decide which one it refers to.  
- A verb can attend to its subject, objects, and modifiers.  
- Tokens that are far apart in the sentence (or document) can still strongly influence each other.

Compared to RNNs:

- No need to pass information step-by-step through many time steps.  
- Information can travel across the sequence in **one attention hop**.

---

## Self-Attention: Slightly More Formal

For each position in the sequence, the model computes three vectors:

- **Query (Q)** – what this token is “asking” about.  
- **Key (K)** – how this token can be “matched” by others.  
- **Value (V)** – the information this token contributes if it is attended to.

Mechanism (single head, simplified):

1. Compute compatibility scores between the **query** of one token and the **keys** of all tokens.  
2. Scale scores by \( \sqrt{d_k} \) to keep them numerically stable.  
3. Apply **softmax** to turn scores into attention weights (probabilities).  
4. Take a **weighted sum of the values** using these weights.

Result:

- Each token’s new representation is a **blend** of all tokens, weighted by relevance.  
- The model learns where to put its attention during training.

---

## Multi-Head Attention

A single attention pattern might not be enough. **Multi-head attention** runs several attention mechanisms in parallel:

- Each head has its own **Q, K, V projections**.  
- Each head can learn to focus on **different types of relationships**:
  - One head might track **subject–verb agreement**.  
  - Another might track **position in a list**.  
  - Another might capture **coreference** (“he”, “she”, “it”).

Process:

1. Compute attention separately for each head.  
2. Concatenate all head outputs.  
3. Pass through a final linear layer to mix them.

This gives the model a richer, multi-perspective view of the sequence at each layer.

---

## Positional Encodings

Because Transformers **do not process tokens sequentially** like RNNs, they need a way to know **which token is first, second, third, etc.**

Solution: **positional encodings / embeddings**.

- For each position index \(i\), we add (or concatenate) a **position vector** to the token embedding.  
- This gives the model information about **order** and **relative positions**.

Two common approaches:

- **Sinusoidal positional encodings** (original Transformer paper):  
  - Fixed, deterministic functions of position using sine/cosine at different frequencies.  
  - Let the model extrapolate to longer sequences than seen in training (to some extent).  
- **Learned positional embeddings**:  
  - Each position has a learned embedding vector.  
  - Very common in modern models (often with relative position variants).

Key idea: without positional information, the model would see the input as a **bag of tokens**, ignoring order.

---

## Feed-Forward Networks (FFN) and Residual Blocks

After attention, each token’s representation goes through a small **feed-forward network**:

- Typically: Linear → nonlinearity (e.g., GELU/ReLU) → Linear.  
- Applied **independently at each position**, but with shared weights.

Transformer blocks also rely heavily on:

- **Residual (skip) connections**:  
  - Add the input of a sub-layer (attention or FFN) to its output.  
  - Helps gradients flow, stabilizing training of deep networks.  
- **Layer Normalization**:  
  - Normalizes activations per token, improving training stability.

Pattern (one sub-layer):

- `x_out = LayerNorm(x_in + SubLayer(x_in))`

This structure repeats across all blocks and is a big part of why very deep Transformers are trainable.

---

## Encoder–Decoder vs Decoder-Only Transformers

The original Transformer (from “Attention Is All You Need”) was **encoder–decoder**:

- **Encoder**: processes the input sequence (e.g., a sentence in English).  
- **Decoder**: generates output sequence (e.g., translated sentence in French), attending to encoder outputs.

Modern LLMs (like GPT-style models) are usually **decoder-only**:

- Only the decoder stack is used, with **causal (masked) self-attention**:  
  - Each position can only attend to **previous** positions, not future ones.  
  - This enforces an **autoregressive** generation process.

Why decoder-only is popular for LLMs:

- Simpler architecture and training pipeline.  
- Naturally suited to **next-token prediction** on massive text corpora.  
- Flexible for many tasks via prompting (few-shot, instructions, etc.).

---

## Scaling Up Transformers

Transformers scale impressively with:

- **Model size** (parameters).  
- **Dataset size** (tokens).  
- **Compute budget** (training steps).

Empirical findings (from scaling law research):

- As you scale all three in tandem, performance tends to **improve smoothly**.  
- LLMs with billions of parameters can capture rich knowledge and reasoning patterns.

However, scaling introduces challenges:

- **Memory and compute cost** for long context windows (attention is \(O(n^2)\) in sequence length).  
- **Training stability** and optimization dynamics at extreme scales.  
- **Inference latency and cost** for real-time applications.

This is why there is a lot of research into:

- More efficient attention (sparse, linear, sliding window, etc.).  
- Quantization, distillation, and other efficiency techniques.

---

## Limitations of Transformers

Despite their power, Transformers are not magical:

- They can **hallucinate** (confidently state false information).  
- They operate on **finite context windows**; very long documents must be chunked or summarized.  
- They **don’t “understand”** in a human sense; they learn statistical patterns from data.  
- They can encode **biases** and **toxicity** present in training data.

Understanding the architecture helps you design:

- Better **prompts** and **guardrails**.  
- Better **evaluation** strategies.  
- More realistic expectations about what LLMs can and cannot do.

add 
## References and Resources

- **Core papers and explainers**:  
  - “Attention Is All You Need” (Vaswani et al., 2017) – original Transformer paper.  
  - At least one **visual / animated explainer** of self-attention and multi-head attention.  
  - A blog or video you like that contrasts **RNNs vs Transformers**.

- **Interactive tools**:  
  - Transformer/attention visualizers that show token-to-token attention links.  
  - Simple playgrounds where you can tweak sequence length and see model behavior.

- **For later days**:  
  - Articles on **efficient attention** and **long-context models**.  
  - Resources on **scaling laws** for LLMs and why bigger models tend to work better.

You can replace these placeholder bullets with your favorite concrete links and tools.