# Day 23 - Model Providers & APIs (OpenRouter, Groq, etc.)

**Theme:** Week 4 - Agents, Tools, and Automation  
**Date:** 2026-04-23

---

## Learning Objectives

By the end of Day 23, you should be able to:

- Compare model providers on **latency**, **cost**, **quality**, and **reliability**.
- Explain routing layers (single provider, fallback, weighted, A/B).
- Design a provider abstraction that reduces vendor lock-in.
- Implement fallback strategies for outages/rate limits.
- Track provider performance using objective metrics.

---

## Why Multi-Provider Strategy Matters

Depending on one provider can create risk in cost, uptime, and capability gaps.

A routing strategy helps you:

- Fail over during incidents.
- Route low-priority traffic to cheaper models.
- Route latency-critical traffic to faster providers.
- Experiment safely with new model options.

Provider flexibility is now part of AI infrastructure maturity.

---

## Comparison Dimensions

- **Quality**: task-level eval results on your dataset.
- **Latency**: p50/p95 response times by region.
- **Cost**: input/output pricing + hidden overhead.
- **Limits**: rate limits, context limits, concurrency caps.
- **Features**: tool calling, JSON mode, multimodal support.
- **Reliability**: error rates, availability, and incident history.

Do not optimize one metric in isolation.

---

## Routing Patterns

- **Primary + fallback**: simplest resilience model.
- **Cost-aware routing**: choose cheapest model meeting SLA.
- **Latency-aware routing**: select fastest provider dynamically.
- **A/B routing**: controlled experiments to compare candidates.
- **Task-based routing**: coding vs summarization vs extraction model mapping.

Start simple, then add sophistication when traffic and requirements justify it.

---

## Operational Best Practices

- Normalize request/response shapes across providers.
- Keep provider-specific adapters isolated.
- Record full telemetry per call (provider/model/version).
- Add automatic circuit breakers for repeated failures.
- Re-run evaluation suites before changing routing defaults.

A routing layer is a product decision engine, not just a proxy.

---

## Suggested Hands-On Exercises

- Implement primary + fallback between two providers.
- Route by task type and compare quality/cost.
- Run a one-week A/B test with fixed prompts.
- Build a dashboard for error rate, p95 latency, and unit cost.

---

## What Comes Next

- **Day 24 - Fine-tuning & PEFT**: when adaptation beats pure prompting/RAG.

---

## References and Resources

- Provider docs for pricing and API limits.
- OpenRouter/Groq and multi-provider routing guides.
- Engineering posts on resilient inference routing.
