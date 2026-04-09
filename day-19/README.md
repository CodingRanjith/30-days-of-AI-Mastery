# Day 19 - AI Agents

**Theme:** Week 4 - Agents, Tools, and Automation  
**Date:** 2026-04-19

---

## Learning Objectives

By the end of Day 19, you should be able to:

- Explain how an AI agent differs from a single prompt-response LLM call.
- Describe the **plan -> act -> observe -> reflect** loop.
- Classify agent patterns: reactive, planning, and multi-agent collaboration.
- Design simple stopping criteria to avoid runaway loops.
- Evaluate where agents create value vs where deterministic workflows are better.

---

## What Makes a System an "Agent"?

An agent is a model-driven system that can:

- Maintain a goal over multiple steps.
- Choose actions/tools based on intermediate results.
- Update its strategy from observations.
- Continue until a stop condition is met.

This iterative behavior is what makes agents feel autonomous.

---

## Core Agent Loop

1. **Plan**: interpret objective and decide next step.
2. **Act**: call tool/API or perform a subtask.
3. **Observe**: ingest tool output or environment feedback.
4. **Reflect**: adjust plan and decide next action.
5. Repeat until completion, failure, or timeout.

Reliable agents are less about smart prompts and more about strong loop control.

---

## Agent Architectures (Practical View)

- **Reactive agent**: picks next action from current context only.
- **Planning agent**: generates explicit subgoals before acting.
- **Supervisor + workers**: coordinator delegates to specialized subagents.
- **Event-driven agents**: triggered by external system events/webhooks.

Choose the simplest architecture that satisfies your task complexity.

---

## Where Agents Work Well

- Multi-step research and synthesis.
- Tool-heavy tasks with uncertain paths.
- Workflow automation that needs adaptive branching.
- Long-running operations with checkpoints.

Where they underperform:

- Strictly deterministic business logic.
- High-risk actions without guardrails/approvals.
- Real-time low-latency paths where loops are too expensive.

---

## Reliability Patterns

- Set max iterations and per-step timeouts.
- Force structured outputs for action decisions.
- Add tool-call validation before execution.
- Keep execution traces for replay/debugging.
- Introduce human approval for high-impact actions.

Agent quality depends on orchestration quality as much as model quality.

---

## Suggested Hands-On Exercises

- Implement a tiny agent that plans a 3-step task with one search tool.
- Add loop limits and compare outcomes with/without limits.
- Create a supervisor-worker toy example (researcher + summarizer).
- Log every action and inspect one failed run to identify the root cause.

---

## What Comes Next

- **Day 20 - Memory Systems**: making agents consistent over long interactions.

---

## References and Resources

- Agent framework docs and architecture guides.
- Papers/blogs on planning and tool-using LLMs.
- Practical postmortems on agent failure modes.
