# Day 21 - Tool Calling & APIs

**Theme:** Week 4 - Agents, Tools, and Automation  
**Date:** 2026-04-21

---

## Learning Objectives

By the end of Day 21, you should be able to:

- Define robust tool/function schemas for LLM tool calling.
- Validate model-generated arguments before executing API calls.
- Handle multi-step tool workflows with state and retries.
- Prevent unsafe or unintended tool actions.
- Measure tool-calling reliability with practical metrics.

---

## Why Tool Calling Is Critical

LLMs are strong at reasoning and language, but weak at guaranteed factual execution.

Tool calling lets models:

- Query live systems instead of guessing.
- Trigger real actions through APIs.
- Chain deterministic operations into broader workflows.

This is the bridge from assistant to operator.

---

## Tool Calling Flow

1. Provide tool specs (name, description, JSON schema).
2. Model decides whether to call a tool.
3. System validates parameters.
4. Tool/API executes.
5. Result returns to model.
6. Model continues reasoning or finalizes response.

Never execute raw model output without validation and policy checks.

---

## Tool Schema Design Tips

- Keep schema fields explicit and typed.
- Include enums/ranges where possible.
- Separate optional vs required fields.
- Use concise descriptions with examples.
- Prefer small, focused tools over one giant endpoint.

Clear schemas reduce hallucinated arguments and malformed calls.

---

## Reliability and Safety Patterns

- Pre-execution validation and normalization.
- Idempotency keys for retry-safe actions.
- Timeouts, retries, circuit breakers.
- Role-based authorization before side effects.
- Human confirmation for destructive actions.

Treat tool execution as production API engineering, not prompt engineering.

---

## Multi-Step Workflow Handling

- Persist workflow state between calls.
- Track tool outputs and intermediate decisions.
- Detect dead loops or repeated failed actions.
- Provide recovery prompts when tools fail.

State-aware orchestration is essential for complex automation.

---

## Suggested Hands-On Exercises

- Define 3 tools (search, get-record, update-record) with strict schemas.
- Inject malformed arguments and test validation behavior.
- Add retry + timeout policies and compare success rates.
- Create one approval-required tool path and verify guardrails.

---

## What Comes Next

- **Day 22 - Workflow Automation with n8n**: connecting model steps to business process automation.

---

## References and Resources

- Function/tool calling docs from model providers.
- API reliability patterns (timeouts, retries, idempotency).
- Safety checklists for action-taking agents.
