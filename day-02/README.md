# Day 02 – LLM Fundamentals

**Theme:** Week 1 – Core Foundations (AI + LLM Internals)  
**Date:** 2026-04-02

---

## Learning Objectives

By the end of Day 2, you should be able to:

- Explain what a Large Language Model (LLM) is in precise but simple terms.  
- Describe how text is turned into tokens and processed by an LLM.  
- Understand what a context window is and why it matters.  
- Use temperature and top-p intuitively to control randomness vs reliability.  

---

## What is a Large Language Model?

At a high level:

- An **LLM** is a model that predicts the **next token** given previous tokens.  
- It is trained on massive text corpora to learn patterns of language, facts, and reasoning structures.  
- Internally it is usually a large **Transformer** with billions of parameters.

You can think of an LLM as a very advanced **auto-complete engine** that has learned:

- Grammar, style, and writing patterns.  
- Associations between concepts (e.g., “Paris” ↔ “France”).  
- Common reasoning patterns (step-by-step explanations, code, SQL, etc.).  

Key idea: the model is not “looking things up” in a database; it is **generating** text based on patterns it has learned from data.

---

## Tokens and Tokenization (Quick Recap)

LLMs do not work directly on raw characters or words. They operate on **tokens**.

- A **token** is a small unit of text (might be a character, subword, or word fragment).  
- Tokenization maps text → list of token IDs, which the model can embed into vectors.

Example (illustrative only):

- Text: `Hello world!`  
- Tokens: `["Hello", " world", "!"]` → `[1234, 5678, 901]`

Important points:

- Different models have different tokenizers.  
- Tokenization affects **length**, **cost**, and **how text is split** (important for prompts and RAG).

You explored tokenization more deeply on Day 5; here we just need the concepts to understand LLM behavior.

---

## Context Window

The **context window** is how many tokens the model can see at once for a single request.

- If a model has a 4k token context window, it can only consider roughly 4,000 tokens of prompt + output at a time.  
- Newer models can handle 8k, 16k, 128k or even more tokens.

Why this matters:

- **Long prompts**: If your prompt is too long, it may get truncated or rejected.  
- **Conversation history**: In chat, the full history is often truncated to fit into the context window.  
- **RAG**: Retrieved documents plus instructions must all fit into the window.

You can think of the context window as the model’s **short-term memory** for that request.

---

## How LLM Inference Works (High Level)

1. **Input text** is tokenized into IDs.  
2. Tokens are mapped to **embeddings** (vectors).  
3. The Transformer processes the sequence, layer by layer, using self-attention.  
4. The model outputs a **probability distribution** over the next token.  
5. A decoding strategy (greedy, sampling, beam search, etc.) chooses the actual next token.  
6. That token is appended to the sequence, and the process repeats until stopping.

The key is: the model never “chooses” full sentences at once. It makes **many small next-token decisions**, guided by probabilities.

---

## Decoding, Temperature, and Top-p

Once we have a probability distribution over next tokens, we must decide **how to sample** from it.  
This is where **temperature** and **top-p** come in.

### Temperature

- Temperature is a scalar that **scales the logits** (pre-softmax scores) before converting them to probabilities.  
- **Low temperature (e.g., 0–0.3)**:
  - Distribution becomes sharper.  
  - Model tends to pick the most likely token.  
  - Outputs are more **deterministic and factual**, but can be repetitive.  
- **High temperature (e.g., 0.8–1.2)**:
  - Distribution becomes flatter.  
  - Model samples from a wider range of tokens.  
  - Outputs are more **creative and diverse**, but can be less reliable.

Intuition:

- Use **lower temperature** for tasks that need accuracy and consistency (code, SQL, instructions).  
- Use **moderate/higher temperature** for creative tasks (brainstorming, marketing copy, stories).

### Top-p (Nucleus Sampling)

- Instead of taking the full token distribution, **top-p** keeps the smallest set of tokens whose cumulative probability ≥ p.  
- Example: top-p = 0.9:
  - Sort tokens by probability.  
  - Keep adding tokens until cumulative probability ≥ 0.9.  
  - Sample only from this subset.

Effects:

- **Lower top-p (e.g., 0.7)**:
  - Limits sampling to a smaller, high-probability set.  
  - More focused, less random.  
- **Higher top-p (e.g., 0.95–1.0)**:
  - Allows sampling from a larger set.  
  - More diverse, potentially more surprising outputs.

Temperature and top-p can be combined, but many providers recommend **tuning one at a time**.

---

## Prompt Design Basics (For LLM Fundamentals)

To see LLM fundamentals in action, it helps to structure prompts clearly:

- Provide **instructions**: what you want the model to do.  
- Provide **context**: any background, examples, or constraints.  
- Provide **output format**: bullet points, JSON, steps, etc.

Simple example prompt:

> You are an assistant for basic math. Explain your reasoning step by step, then give a final answer at the end under the heading "Final Answer".

Even at this early stage, you should notice how **clear prompts** lead to more predictable behavior.

---

## References and Resources

- Concept explanations:
  - Introductory article or video explaining what LLMs are and how they work.  
  - Resource that visualizes tokenization and context windows.  
- Parameters and decoding:
  - Documentation page from a model provider explaining temperature and top-p.  
- For upcoming days:
  - Any good guide you like on transformers and attention (used on Day 3).

You can replace these placeholder bullets with specific links you prefer.
