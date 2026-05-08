---
date: 2026-05-08
topic: Agent Architecture and Design
score: pending
---

# Answers — 2026-05-08
## Topic: Agent Architecture and Design

---

## My Answers
Q1: 
Q2: 
Q3: 
Q4: 
Q5: 
Q6: 
Q7: 
Q8: 
Q9: 
Q10: 

## Notes During Session:

## Score: /10

---

## Correct Answers
Q1: C — Planning is the stage where the agent reasons about inputs and decides which action to take next.
Q2: D — No cross-invocation memory + horizontal scaling = stateless agent; reactive describes response style, not statefulness.
Q3: B — The orchestrator's job is task decomposition and delegation; workers are specialized executors.
Q4: B — The schema (especially the description field) is what the LLM uses to decide when and how to call a tool.
Q5: B — Fixed P→A→C→E sequence with Evaluator re-routing is a sequential pipeline with a feedback loop, not peer-to-peer or hierarchical.
Q6: B — Perceive (retrieval), Plan (summarize), Act (write to EHR) are all there; Reflect (quality check) is missing.
Q7: C — Parallel fan-out across 10 domains then aggregation is exactly the orchestrator-worker pattern; pipeline would be sequential.
Q8: B — LLMs use tool descriptions to decide when to invoke a tool; vague or missing descriptions cause the model to skip it.
Q9: C — Without external checkpointing of intermediate state, a crash loses all progress; the agent needs persistence outside its process.
Q10: C — Pipeline's fixed order is easy to trace/debug but inherently sequential; orchestrator-worker enables parallelism at the cost of coordination complexity.
