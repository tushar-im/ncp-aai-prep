---
date: 2026-05-09
topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AgentIQ
score: pending
---

# Answers — 2026-05-09
## Topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AgentIQ

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
Q1: B — NIM is a containerized, OpenAI-API-compatible model inference endpoint with NVIDIA optimizations (TensorRT-LLM, quantization) baked in.
Q2: C — NeMo Guardrails policies are written in Colang, NVIDIA's own declarative DSL for rail logic.
Q3: C — Triton supports all major backends: TensorRT-LLM, ONNX, PyTorch, TF, Python custom, and vLLM — that's the point, it's backend-agnostic.
Q4: B — TensorRT-LLM is an optimization/compilation library, not a server; you compile a model with it then serve via Triton.
Q5: C — TensorRT-LLM directly addresses both problems: quantization reduces memory footprint, paged KV cache handles long-context memory spikes.
Q6: B — Triton model ensemble pipelines chain models natively inside Triton, eliminating external HTTP calls between retrieval, reranking, and LLM.
Q7: C — NeMo Guardrails with input/output rails in Colang is exactly the right tool: declarative, model-agnostic, configurable without model redeployment.
Q8: B — The bottleneck is the local LLM call in the Coder agent; TensorRT-LLM optimizes it and Triton serves it with dynamic batching to reduce per-call latency.
Q9: B — NIM wraps the underlying model as-is; a different model (e.g. Llama instead of GPT-4) may have different tool-calling behavior or fine-tuning even with an identical API surface.
Q10: C — Tensor parallelism shards weight matrices across GPUs so each holds a fraction of every layer — the standard approach for models too large for one GPU. (D is also valid but tensor parallelism is preferred for same-layer parallelism; pipeline parallelism adds pipeline bubbles.)
