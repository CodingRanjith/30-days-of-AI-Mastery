# Day 26 - Deployment (FastAPI, APIs, Cloud)

**Theme:** Week 5 - Adaptation, Ops, and Production Excellence  
**Date:** 2026-04-26

---

## Learning Objectives

By the end of Day 26, you should be able to:

- Package AI logic behind reliable API endpoints.
- Design FastAPI request/response schemas for model and RAG services.
- Apply production basics: auth, rate limiting, logging, and retries.
- Plan deployment topologies for cloud scaling.
- Define SLOs for latency, availability, and error budgets.

---

## From Notebook to Service

Production AI requires service boundaries, not ad hoc scripts.

A deployable service should include:

- Stable API contract.
- Model/routing configuration controls.
- Observability hooks.
- Security and abuse protections.

Deployment quality often determines product reliability more than model quality.

---

## FastAPI Service Skeleton (Conceptual)

- `/health` for liveness/readiness.
- `/infer` for synchronous generation.
- `/rag` for retrieval-augmented responses.
- `/metrics` for monitoring stack ingestion.

Use typed schemas and explicit validation for all user input.

---

## Cloud Deployment Considerations

- Containerize service for reproducible runtime.
- Externalize secrets and provider keys.
- Horizontal scale for burst traffic.
- Queue/background workers for long-running tasks.
- Regional deployment strategy for latency.

Plan infra based on traffic profile, not just average load.

---

## Security and Reliability Basics

- JWT/API-key auth and tenant isolation.
- Request size and token budget caps.
- Timeout and retry policies.
- Structured error responses.
- Redaction of sensitive data in logs.

A secure API is mandatory when model output can trigger business actions.

---

## Suggested Hands-On Exercises

- Build a small FastAPI endpoint wrapping one LLM call.
- Add pydantic schema validation and error handling.
- Add simple rate limiting and request logging.
- Deploy to one cloud target and test p95 latency under load.

---

## What Comes Next

- **Day 27 - LLMOps**: operating AI systems after deployment.

---

## References and Resources

- FastAPI and pydantic docs.
- Cloud deployment guides for containerized APIs.
- Reliability and API security best-practice checklists.
