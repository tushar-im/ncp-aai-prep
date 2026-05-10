---
date: 2026-05-10
topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AIQ Toolkit
score: pending
---

# Answers — 2026-05-10
## Topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AIQ Toolkit

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
Q1: C — NIM is the containerized, OpenAI-compatible endpoint that hides all Triton and TensorRT-LLM configuration from the developer.
Q2: B — Colang is the purpose-built DSL for writing NeMo Guardrails policies and conversation flows.
Q3: C — Triton is the right tool for multi-backend, multi-model concurrent serving; NIM is opinionated and LLM-only.
Q4: C — In-flight batching continuously refills slots as sequences complete rather than waiting for a full batch to finish, which is the key LLM throughput optimization.
Q5: B — NIM exposes an OpenAI-compatible API, so swapping the base URL is all that's needed; no SDK or chain rewrite required.
Q6: B — An output rail intercepts the LLM response (Author's output) before it propagates downstream to the Evaluator.
Q7: C — AIQ Toolkit's profiler captures per-step latency, token usage, and tool call frequency — operational metrics for the agent workflow, not accuracy metrics.
Q8: B — Model ensembles in Triton chain multiple models into a single pipeline executed as one API call; this is the designed use case.
Q9: B — Retrieval rails filter knowledge chunks before they enter the LLM prompt, allowing policy-based filtering of retrieved content.
Q10: B — Low GPU utilization between requests is a batching problem; in-flight batching in TensorRT-LLM (which NIM uses internally) addresses this by continuously filling available compute slots.
