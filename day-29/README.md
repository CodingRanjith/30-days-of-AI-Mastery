# Day 29 - AI Safety & Guardrails

**Theme:** Week 5 - Adaptation, Ops, and Production Excellence  
**Date:** 2026-04-29

---

## Learning Objectives

By the end of Day 29, you should be able to:

- Identify key AI risk categories: harmful output, bias, prompt injection, unsafe tool use.
- Design layered guardrails across prompt, retrieval, model, and action layers.
- Apply validation and policy checks before executing external actions.
- Define safe failure modes and escalation paths.
- Build a safety monitoring loop for post-deployment governance.

---

## Why Guardrails Are Essential

Safety is not one filter; it is a system property.

Without guardrails, AI systems can:

- Produce harmful or policy-violating output.
- Leak sensitive data.
- Execute risky actions from malicious prompts.
- Drift into unsafe behavior over time.

Responsible deployment requires defense in depth.

---

## Risk Layers to Address

- **Input risks**: jailbreaks, prompt injection, adversarial data.
- **Retrieval risks**: poisoned corpora, untrusted sources.
- **Generation risks**: hallucinations, unsafe recommendations.
- **Tool risks**: unauthorized actions, parameter abuse.
- **Output risks**: compliance violations and harmful content.

Each layer needs explicit controls.

---

## Practical Guardrail Stack

1. Input sanitization and policy pre-checks.
2. System prompt policies and constrained response formats.
3. Retrieval source filtering and trust scoring.
4. Output moderation/classification checks.
5. Action gating with role-based approvals.
6. Audit logs and incident response playbooks.

Layered safeguards are more robust than any single model-level control.

---

## Prompt Injection Defense Basics

- Separate instructions from untrusted retrieved content.
- Use strict tool allowlists and schema validation.
- Ignore/flag attempts to override system policies.
- Add source citation requirements for sensitive answers.
- Require human confirmation for high-impact actions.

Assume untrusted context can be adversarial.

---

## Suggested Hands-On Exercises

- Build a simple safety policy matrix (allowed/blocked/escalate).
- Test with adversarial prompts and log failure modes.
- Add output moderation and compare false positives/negatives.
- Add approval gate before one destructive tool action.

---

## What Comes Next

- **Day 30 - Cost & Performance Optimization**: optimizing systems without sacrificing quality and safety.

---

## References and Resources

- OWASP-style guidance for LLM application security.
- Prompt injection and safety engineering articles.
- Governance frameworks for responsible AI operations.
