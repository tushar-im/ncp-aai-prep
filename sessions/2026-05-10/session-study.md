---
date: 2026-05-10
topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AIQ Toolkit
sections:
  - NVIDIA Platform Implementation
weight: 7%
phase: 1
day: 5
days_until_exam: 56
status: pending
---

# Study Session — 2026-05-10
## Topic: NVIDIA Platform Overview — NIM, NeMo Guardrails, Triton, AIQ Toolkit
## Exam Section: NVIDIA Platform Implementation (7% of exam)
## Estimated Time: 1 hour

---

## Concept Brief

The NVIDIA platform for agentic AI is a stack of four tools that address distinct problems: where does the model run, how is it optimized, how do you keep it safe, and how do you build agents around it. At this stage you need to know what each tool does, when you'd use it, and how they relate to each other. Deep configuration comes in Phase 2.

**NIM — NVIDIA Inference Microservices**

NIM is a containerized, pre-packaged inference endpoint for foundation models. You pull a NIM container from the NVIDIA catalog, point it at your GPU, and get an OpenAI-compatible REST API. Under the hood it runs TensorRT-LLM and Triton, but you never touch those layers directly. The value prop: swap your OpenAI API call to a self-hosted NIM endpoint with a single URL change — same SDK, same request format.

For your RAG pipeline on Bedrock, NIM would let you run the same Llama or Mistral model on your own infrastructure without rewriting any of the LangChain chains. That's the integration story: drop-in replacement. NIM handles model loading, quantization, batching, and health checks. You don't configure any of it.

**TensorRT-LLM — NVIDIA's LLM Inference Optimizer**

TensorRT-LLM is the engine NIM runs internally. It's a compiler and runtime that takes a model checkpoint (HuggingFace format) and optimizes it for NVIDIA GPUs: kernel fusion, quantization (INT4/INT8/FP8), paged KV cache, and in-flight batching. In-flight batching is the key performance concept: instead of waiting for all requests in a batch to finish before starting new ones, the engine continuously fills slots as sequences complete. At high concurrency, this significantly improves GPU utilization.

You don't need to call TensorRT-LLM directly if you use NIM. But the exam tests whether you know what it does and why it matters for latency and throughput at scale.

**Triton Inference Server**

Triton is NVIDIA's model serving platform — more flexible and lower-level than NIM. It supports multiple backends: TensorRT, TensorRT-LLM, PyTorch, TensorFlow, ONNX, Python custom. Models are loaded from a model repository (a structured directory with a config.pbtxt per model). Triton handles concurrent model execution, dynamic batching, model ensembles (chaining multiple models in a single inference pipeline), and gRPC/HTTP endpoints.

Use Triton when you need flexibility NIM doesn't provide: serving non-LLM models, building a multi-model pipeline (e.g., ASR → LLM → TTS), or running custom Python inference logic. NIM wraps Triton; if NIM is too opinionated for your use case, drop down to Triton.

For your de-id.org work, a Triton backend could serve the ONNX-exported Stanford NER and PaddleOCR models with dynamic batching — similar to what you're doing with ONNX Runtime Web, but server-side at scale.

**NeMo Guardrails**

NeMo Guardrails is an open-source toolkit for adding programmable safety and compliance behavior to LLM applications. It intercepts calls between your application and the LLM, applies policy rules, and either allows, modifies, or blocks the interaction.

Guardrails are written in Colang, a purpose-built DSL that looks like simplified conversation flow scripting. You define "flows" — if the user says X, do Y. Four rail types:
- **Input rails**: evaluate and optionally block/modify user input before it reaches the LLM
- **Dialog rails**: enforce conversation structure and topic scope
- **Output rails**: evaluate and optionally block/modify LLM responses before they reach the user
- **Retrieval rails**: intercept and filter retrieved chunks before they're inserted into the prompt

For PACE: NeMo Guardrails could wrap the Author or Coder agents with output rails that catch compliance-violating text before it reaches the Evaluator — reducing wasted evaluation cycles. For your Bedrock clinical assistant, input rails could block users from asking the model to act as a prescribing physician.

**AIQ Toolkit (Agent Intelligence Toolkit)**

AIQ Toolkit is NVIDIA's framework for building, profiling, and evaluating multi-agent workflows. It provides a workflow engine (define agent graphs), a profiler (captures latency, token counts, tool call frequency per agent step), and an evaluator (measures output quality). It supports LangChain, LlamaIndex, and custom agents as workflow nodes.

The key differentiator vs LangGraph or CrewAI: built-in profiling is first-class. Every run captures exactly where time and tokens are spent. For a PACE-like pipeline, AIQ would surface that the Evaluator call is taking 40% of the total latency — actionable signal for optimization.

---

## Sub-topics Covered

- **6.1 NIM microservices for model deployment** — REVIEW (know the concept, haven't deployed NIM)
- **6.2 TensorRT-LLM for inference optimization** — REVIEW (understand in-flight batching and quantization conceptually)
- **6.3 Triton Inference Server for model serving** — REVIEW (know the architecture, used ONNX Runtime Web which is adjacent)
- **6.4 NeMo Guardrails for safety and alignment** — REVIEW (know the pattern from PACE Evaluator, DSL is new)
- **6.5 AIQ Toolkit for agent development and evaluation** — NEW (no prior hands-on)

---

## Key Points to Remember

- NIM = containerized LLM microservice, OpenAI-compatible API, wraps TensorRT-LLM + Triton internally — drop-in replacement for hosted APIs
- TensorRT-LLM delivers throughput via **in-flight batching**: slots are refilled continuously as sequences complete, not batch-by-batch
- Triton is framework-agnostic (TF, PyTorch, ONNX, TensorRT, Python) and supports **model ensembles** — NIM is higher-level and opinionated
- NeMo Guardrails uses **Colang** DSL and has four rail types: input, dialog, output, retrieval
- **Output rails** catch policy violations after the LLM generates text but before the user sees it; **retrieval rails** filter knowledge chunks before they enter the prompt
- AIQ Toolkit's primary differentiator is **first-class profiling** — per-step latency and token usage captured automatically in every agent run
- NIM is the right choice when you want a self-hosted drop-in; Triton is the right choice when you need multi-model flexibility or custom backends

---

## Go Deeper

`study-brain/ragas/nvidia_metrics.md` — Contains NVIDIA-specific evaluation metrics. Read the metric definitions and connect them to how AIQ Toolkit's evaluator measures agent output quality.

`study-brain/ragas/agents.md` — Read for how agentic metrics are structured. NVIDIA's AIQ Toolkit eval is conceptually similar — this gives you the vocabulary for Q8-Q10 style questions.

Official NVIDIA docs to add to study-brain (no local copy yet — queue for next session):
- NIM catalog: docs.nvidia.com/nim — read the quickstart only
- NeMo Guardrails: docs.nvidia.com/nemo-guardrails — read the Colang intro and rail types page

---

## Cheatsheet

**NIM** — NVIDIA Inference Microservice; containerized LLM endpoint with OpenAI-compatible API; wraps TensorRT-LLM + Triton; drop-in for hosted API calls

**TensorRT-LLM** — NVIDIA's LLM compiler and runtime; applies quantization, kernel fusion, paged KV cache; provides in-flight batching for high-throughput serving

**In-flight batching** — TensorRT-LLM feature that continuously refills batch slots as sequences complete, rather than waiting for the full batch to finish; key LLM throughput technique

**Triton Inference Server** — NVIDIA's model serving platform; framework-agnostic (TF, PyTorch, ONNX, TensorRT, Python); supports dynamic batching, concurrent models, and model ensembles

**Model ensemble (Triton)** — chaining multiple models into a single Triton pipeline (e.g., ASR → LLM → TTS) executed as one inference call

**NeMo Guardrails** — open-source safety middleware for LLM apps; intercepts input and output flows; rules written in Colang DSL

**Colang** — NeMo Guardrails' DSL for defining conversation flows and safety policies; used to specify what the LLM can and cannot do

**Four rail types** — Input (pre-LLM user message), Dialog (conversation structure), Output (post-LLM response), Retrieval (knowledge chunks before prompt injection)

**AIQ Toolkit** — NVIDIA's agent framework; provides workflow engine, built-in profiler (latency/tokens/tool calls per step), and evaluator; integrates with LangChain and LlamaIndex

**Dynamic batching (Triton)** — server-side batching that groups individual requests arriving at different times into a single model inference batch to maximize GPU utilization

---

## Exam Questions

Q1: Which NVIDIA tool provides a containerized, OpenAI-compatible REST endpoint for LLM inference that requires no Triton or TensorRT-LLM configuration from the developer?
A) Triton Inference Server
B) AIQ Toolkit
C) NIM (NVIDIA Inference Microservices)
D) NeMo Guardrails

Q2: NeMo Guardrails policies are written in a purpose-built DSL. What is it called?
A) GuardScript
B) Colang
C) NemoFlow
D) SafeYAML

Q3: A team needs to serve three models simultaneously — a 7B LLM, an ONNX speech-to-text model, and a custom Python re-ranking model — with dynamic batching on a single GPU server. Which NVIDIA tool is the best fit?
A) NIM — deploy three separate containers
B) TensorRT-LLM — compile all three into a single engine
C) Triton Inference Server — use multi-backend concurrent model execution
D) AIQ Toolkit — route requests across agents

Q4: TensorRT-LLM's in-flight batching improves LLM throughput by:
A) Pre-compiling multiple model variants and switching between them based on request length
B) Caching token embeddings across users to avoid recomputation
C) Continuously filling batch slots as sequences complete rather than waiting for the full batch to finish
D) Splitting long sequences across multiple GPUs in pipeline parallelism

Q5: You are integrating a clinical AI assistant built on your existing LangChain + OpenAI SDK stack. You want to switch to a self-hosted Llama 3 70B model with the smallest possible code change. Which NVIDIA tool is the right choice and why?
A) Triton Inference Server — it natively supports LangChain callbacks
B) NIM — it exposes an OpenAI-compatible endpoint, so only the base URL changes
C) TensorRT-LLM Python API — it gives the most direct access to the optimized model
D) AIQ Toolkit — it wraps LangChain natively and abstracts the model layer

Q6: Your PACE compliance pipeline has a known issue: the Evaluator sometimes approves Author outputs that contain subtle HIPAA violations because the violation is buried in the middle of a long response. Which NVIDIA tool and rail type would intercept this before the Evaluator scores it?
A) NeMo Guardrails with an input rail — block the Author's prompt before it reaches the LLM
B) NeMo Guardrails with an output rail — intercept the Author's LLM response before it reaches the Evaluator
C) NeMo Guardrails with a retrieval rail — filter retrieved HIPAA policy chunks
D) Triton Inference Server ensemble — route Author output through a classifier model first

Q7: The AIQ Toolkit profiler captures data on a 4-agent pipeline run. Which output is it specifically designed to produce?
A) Accuracy scores comparing agent output to a ground-truth dataset
B) A flame graph of GPU kernel execution times per model layer
C) Per-step latency, token counts, and tool call frequency across the agent workflow
D) A diff of the agent's reasoning trace against a reference reasoning chain

Q8: A developer using Triton wants to build a pipeline where audio is transcribed by a Whisper model, the transcript is passed to a Llama model for summarization, and the summary is passed to a BERT model for entity extraction — all in a single API call. Which Triton feature enables this?
A) Dynamic batching — group all three model requests into one batch
B) Model ensemble — chain the three models as a single Triton pipeline
C) Concurrent model execution — run all three models in parallel on the same GPU
D) BLS (Business Logic Scripting) — write custom routing logic between models

Q9: NeMo Guardrails retrieval rails are added to a RAG-based medical chatbot. What specifically do they do?
A) Validate that the LLM's cited sources match the retrieved documents
B) Filter retrieved knowledge chunks before they are inserted into the LLM prompt, blocking chunks that violate policy
C) Rate-limit retrieval calls to prevent the chatbot from querying the vector store too frequently
D) Rerank retrieved documents by clinical relevance before passing them to the LLM

Q10: You are running a self-hosted Llama 3 70B in production using NIM. Query latency is acceptable but throughput is too low — the GPU is underutilized between requests. The root cause is most likely:
A) NIM is not using TensorRT-LLM internally, so quantization is not being applied
B) The NIM container is serving requests sequentially rather than batching — enabling in-flight batching in TensorRT-LLM would address this
C) NIM's OpenAI-compatible API adds overhead versus a direct gRPC Triton call
D) The model needs to be recompiled with a higher max batch size using the TensorRT-LLM Python API directly
