---
date: 2026-05-09
topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AgentIQ
sections:
  - NVIDIA Platform Implementation
weight: 7%
phase: 1
day: 3
days_until_exam: 57
status: pending
---

# Study Session — 2026-05-09
## Topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AgentIQ
## Exam Section: NVIDIA Platform Implementation (7% of exam)
## Estimated Time: 1 hour

---

## Concept Brief

This is a Phase 1 orientation session. Goal: build a working mental model of each NVIDIA platform tool so you can answer "what does X do and when would you use it" for any of them. No deep configuration yet.

**NVIDIA NIM (Inference Microservices)**

NIM packages optimized LLMs and other AI models as OCI-compliant containers you can self-host or pull from NVIDIA GPU Cloud (NGC). Each NIM exposes an OpenAI-compatible REST API, so if your code works with GPT-4 via OpenAI SDK, it works with a NIM out of the box — just swap the base URL. NVIDIA bakes in TensorRT-LLM optimizations, quantization, and batching automatically. Your AWS Bedrock Claude assistant is architecturally similar but on NVIDIA's stack instead of Amazon's: managed inference endpoint, swappable underlying model, same API surface. The exam cares about: NIMs are per-model containers, they're GPU-optimized by default, they can run on-prem or in cloud, and they're the NVIDIA answer to "how do I serve a model without building serving infrastructure."

**NeMo Guardrails**

Programmable safety layer for LLM applications, written in Colang — NVIDIA's own DSL. Three rail types matter: input rails (filter what the user sends before it hits the LLM), output rails (filter or rewrite what the LLM sends back), and dialog rails (enforce conversational flow, like preventing the LLM from role-playing as another model). Guardrails wrap any LLM — you can put them in front of a NIM, OpenAI, Bedrock, anything. In PACE, your Evaluator agent effectively implements output rails manually: it checks whether the Author/Coder output meets compliance criteria. NeMo Guardrails would let you do that declaratively in Colang rather than in Python. Key exam distinction: Guardrails are for behavioral safety (what the model says), not model-level safety (alignment, RLHF). They operate at the application layer.

**Triton Inference Server**

NVIDIA's open-source model serving framework. Think of it as the infrastructure layer below NIM — NIM is a pre-packaged Triton deployment for a specific model. Triton supports multiple backends simultaneously: TensorRT-LLM, ONNX Runtime, TensorFlow, PyTorch, Python (custom), and vLLM. Key capabilities: dynamic batching (accumulates requests and batches them to maximize GPU utilization), concurrent model execution (run multiple models on the same GPU), model ensemble pipelines (chain models via Triton natively), and a model repository (file system structure that Triton polls for model updates). Triton exposes HTTP and gRPC. Your RAG pipeline retrieval + reranking step would be a natural model ensemble in Triton: retriever model → reranker model → LLM, each step served by Triton.

**TensorRT-LLM**

NVIDIA's LLM optimization library — not a serving framework, an optimization toolkit. You use TensorRT-LLM to compile and quantize a model so it runs faster on NVIDIA GPUs, then serve it via Triton. Key optimizations: INT4/INT8/FP8 quantization (smaller weight representation = faster matmuls), paged KV cache (manages GPU memory for long contexts without pre-allocating the full KV cache), in-flight batching (start processing a new request before others finish), and tensor/pipeline parallelism (split large models across multiple GPUs). The exam will not ask you to write TensorRT-LLM code — it asks conceptually: what problem does it solve (inference speed and memory on GPU), and what are its main techniques.

**AgentIQ (NVIDIA Agent Intelligence Toolkit)**

NVIDIA's Python framework for building and profiling multi-agent AI pipelines. Formerly called "NeMo Agent Toolkit" in some docs — know both names. AgentIQ's differentiator is built-in profiling and benchmarking: you get latency breakdowns per agent step, which is useful for the "Evaluation and Tuning" section of the exam. It integrates with LangGraph, CrewAI, and LlamaIndex, so it's not a replacement for those but a NVIDIA-flavored orchestration and observability layer on top. PACE's 4-agent loop (Planner→Author→Coder→Evaluator) is exactly the kind of pipeline AgentIQ would orchestrate and profile.

---

## Sub-topics Covered

These map to the NCP-AAI exam blueprint section on NVIDIA Platform Implementation:

- **NVIDIA NIM microservices — what they are, how they're deployed, API compatibility** — NEW
- **NeMo Guardrails — Colang DSL, rail types (input/output/dialog), use cases** — NEW
- **Triton Inference Server — architecture, backends, batching, model ensembles** — NEW
- **TensorRT-LLM — optimization library, quantization, paged KV cache, parallelism strategies** — NEW
- **AgentIQ (NeMo Agent Toolkit) — multi-agent orchestration and profiling** — NEW
- **How these tools fit together: TensorRT-LLM → Triton → NIM → Application** — NEW

---

## Key Points to Remember

- NIM = containerized, OpenAI-API-compatible model endpoint with NVIDIA optimizations baked in; runs on NGC or self-hosted
- Triton Inference Server is the underlying serving engine; NIM is a pre-built Triton deployment for a specific model
- NeMo Guardrails operates at the application layer in Colang; three rail types: input, output, dialog
- TensorRT-LLM is an optimization library (not a server); its key techniques are quantization (INT4/INT8/FP8), paged KV cache, and tensor parallelism
- AgentIQ / NeMo Agent Toolkit is NVIDIA's multi-agent framework with built-in profiling — know both names
- Stack order: TensorRT-LLM (optimize) → Triton (serve) → NIM (package) → your app + Guardrails (apply safety)
- Colang is NVIDIA's DSL for writing guardrail logic — it's declarative, not Python

---

## Go Deeper

study-brain/ doesn't have NVIDIA platform files yet. Add these before Week 3:

1. **study-brain/ragas/nvidia_metrics.md** — Covers NVIDIA's Answer Accuracy and Context Relevance metrics used in AgentIQ eval pipelines. Read the "How It's Calculated" sections to understand dual-LLM-judge patterns — these appear in the Evaluation section of the exam.

2. **[Add] study-brain/nvidia/nim-overview.md** — Download from docs.nvidia.com/nim. Focus on: NIM container structure, NGC registry, how to swap base_url in OpenAI SDK to use a NIM. This file doesn't exist yet.

3. **[Add] study-brain/nvidia/nemo-guardrails.md** — Download from docs.nvidia.com/nemo-guardrails. Focus on: Colang syntax basics, the three rail types, how to wrap an existing LLM. This file doesn't exist yet.

---

## Cheatsheet

**NIM** — Containerized NVIDIA-optimized model endpoint; OpenAI-API-compatible; served from NGC or self-hosted GPU

**Triton Inference Server** — Multi-backend model serving framework (TensorRT-LLM, ONNX, PyTorch, Python); supports dynamic batching + model ensembles

**TensorRT-LLM** — LLM inference optimization library; key features: INT4/INT8/FP8 quantization, paged KV cache, tensor/pipeline parallelism

**Paged KV cache** — Manages GPU memory for KV attention states in chunks (like virtual memory paging); prevents OOM for long contexts

**NeMo Guardrails** — Programmable LLM safety layer written in Colang; wraps any LLM at the application layer

**Colang** — NVIDIA's DSL for defining guardrail behavior; declarative, not Python

**Three rail types** — Input rails (filter user input), Output rails (filter LLM response), Dialog rails (constrain conversational flow)

**AgentIQ / NeMo Agent Toolkit** — NVIDIA's Python framework for multi-agent pipeline orchestration + profiling; integrates with LangGraph/CrewAI

**Dynamic batching (Triton)** — Accumulates concurrent inference requests and batches them to maximize GPU utilization

**NIM vs Triton** — NIM is a pre-packaged, model-specific deployment built on Triton; Triton is the general-purpose serving engine underneath

---

## Exam Questions

Q1: What is NVIDIA NIM?
A) A fine-tuning framework for large language models on NVIDIA GPUs
B) A containerized, OpenAI-API-compatible model inference endpoint optimized for NVIDIA hardware
C) A monitoring tool for tracking GPU utilization during inference
D) A distributed training library with automatic model parallelism

Q2: Which language or DSL is used to write NeMo Guardrails policies?
A) YAML with Jinja templating
B) Python with decorator-based rule definitions
C) Colang
D) GraphQL schema language

Q3: Triton Inference Server supports which of the following backends? (Pick the best answer)
A) Only TensorRT-LLM and ONNX Runtime
B) Only PyTorch and TensorFlow
C) TensorRT-LLM, ONNX Runtime, PyTorch, TensorFlow, Python custom, and vLLM
D) Only models that have been compiled with TensorRT-LLM first

Q4: TensorRT-LLM is best described as:
A) A serving framework that exposes REST/gRPC endpoints for LLMs
B) An optimization library that compiles and quantizes LLMs for faster GPU inference
C) A container registry for hosting pre-built NVIDIA model images
D) A safety layer that filters LLM inputs and outputs

Q5: Your team deploys a clinical summarization pipeline using a fine-tuned Llama model. Latency is too high and GPU memory is frequently exhausted on long patient notes. Which NVIDIA tool directly addresses both issues?
A) NeMo Guardrails — reduces token count before LLM sees the prompt
B) AgentIQ — profiles the pipeline to identify the bottleneck
C) TensorRT-LLM — applies quantization and paged KV cache to reduce memory and speed up inference
D) Triton's dynamic batching — accumulates requests to improve throughput

Q6: You are building a RAG pipeline where a retrieval model and a reranking model must both run on GPU before results reach the LLM. You want to minimize latency and avoid managing inter-model HTTP calls. Which Triton feature best handles this?
A) Multi-model deployment — each model runs independently on separate Triton instances
B) Model ensemble pipelines — chain models natively within Triton, no external HTTP between steps
C) Dynamic batching — merge retrieval and reranking into a single batched call
D) Paged KV cache — allows the retrieval and reranking models to share GPU memory

Q7: A team wants to ensure their customer-facing LLM application never discusses competitors and always responds in a specific structured format. They want these rules to be configurable without redeploying the underlying model. Which NVIDIA tool is the right fit?
A) TensorRT-LLM — apply constraints at the compilation step
B) Triton Inference Server — configure model repository constraints via config.pbtxt
C) NeMo Guardrails — define input/output rails in Colang to enforce topic and format constraints
D) AgentIQ — add a guardrail agent as a step in the pipeline

Q8: A PACE-like compliance pipeline runs four agents in sequence. On average, requests take 4 seconds end-to-end. Profiling shows the Coder agent accounts for 2.8 of those seconds. The Coder agent calls a locally-hosted LLM. Which combination of NVIDIA tools would most directly reduce the Coder agent's latency?
A) NeMo Guardrails (reduce output tokens) + Triton dynamic batching (batch Coder requests)
B) TensorRT-LLM (optimize the local LLM) + Triton (serve it with dynamic batching)
C) AgentIQ (profile the Coder agent) + NIM (swap to a NIM endpoint)
D) TensorRT-LLM (optimize) + NeMo Guardrails (reduce retries) + Triton (serve)

Q9: A developer wraps their existing OpenAI-based application to use a self-hosted NIM instead. They change only the `base_url` in their OpenAI client. After the change, they observe that the application sometimes returns responses in a different format and some tool-calling patterns break. What is the most likely explanation?
A) NIM containers do not support the OpenAI function-calling API at all
B) The NIM is using a different underlying model that has different fine-tuning and may handle tool calling differently than GPT-4
C) NIM requires gRPC instead of REST, so HTTP calls are silently downgraded
D) NeMo Guardrails are automatically applied to all NIM deployments and are stripping tool call syntax

Q10: You need to deploy a 70B parameter LLM on a cluster with four A100 80GB GPUs. The model weights alone exceed a single GPU's memory. Which TensorRT-LLM feature should you use, and what does it do?
A) Paged KV cache — partitions the model's attention cache across GPUs to fit within memory
B) INT4 quantization — reduces weights from FP16 to INT4, cutting memory roughly 4x so the model fits on one GPU
C) Tensor parallelism — shards the model's weight matrices across multiple GPUs so each GPU holds a portion of every layer
D) Pipeline parallelism — assigns different transformer layers to different GPUs, reducing per-GPU memory but increasing inter-GPU latency
