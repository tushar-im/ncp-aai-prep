# Study Session — 2026-05-07
## Topic: Evaluation Pipelines and Task Benchmarks
## Exam Section: Evaluation and Tuning (13% of exam)
## Estimated Time: 1 hour

## Concept Brief
An evaluation pipeline for an agentic system is the structured process of 
measuring whether your agent is actually doing what you want — correctly, 
consistently, and at acceptable latency. Think of it like your test suite, 
but for non-deterministic systems.

Your PACE project already does something close to this — the Evaluator agent 
in your Planner→Author→Coder→Evaluator loop checks whether the Coder's output 
satisfies the compliance requirement before re-scanning. That's an eval loop. 
A formal eval pipeline formalizes this pattern across datasets and tracks it 
over time.

The key components: a dataset of test cases (input + expected output), 
a set of metrics (correctness, faithfulness, relevance, latency), 
an execution harness that runs the agent against each test case, 
and a results store that lets you compare runs over time.

For RAG-based agents like your clinical assistant, the standard framework 
is RAGAs — it measures context precision (did you retrieve the right chunks?), 
context recall (did you retrieve all the relevant chunks?), 
faithfulness (does the answer actually follow from the context?), 
and answer relevancy (does the answer address the question?).

Task benchmarks go one level up — instead of measuring a single query, 
you define a full task (e.g., "summarize this patient record and flag 
contraindications") and measure end-to-end success rate across many examples.

## Key Points to Remember
- RAGAs is the standard eval framework for RAG agents — know its 4 metrics
- Evaluation pipelines require a golden dataset — curated input/output pairs
- Always track evals over time — a single score means nothing without a baseline
- Latency-accuracy tradeoff: better retrieval = more chunks = slower response
- NVIDIA Agent Intelligence Toolkit has built-in eval tooling — know it exists

## Study Brain References
- study-brain/nvidia-study-guide.md (section 3: Evaluation and Tuning)

## Exam Questions

Q1: Which RAGAs metric measures whether the generated answer is supported by the retrieved context?
A) Context Precision
B) Context Recall
C) Faithfulness
D) Answer Relevancy

Q2: When building a benchmark for a multi-step agentic task, what is the most important element of your test dataset?
A) Large volume of test cases (>1000)
B) Curated input/output pairs with verified expected outcomes
C) Diversity of LLM models tested against
D) Real-time production traffic samples

Q3: Your RAG pipeline shows high context recall but low faithfulness scores. What does this indicate?
A) The retriever is missing relevant chunks
B) The retrieved chunks are relevant but the LLM is hallucinating answers not supported by them
C) The embedding model needs retraining
D) The vector database index is stale

## Answers
Q1: C — Faithfulness measures whether the answer is grounded in the retrieved context, not invented
Q2: B — Without verified expected outputs you can't measure correctness — volume is secondary
Q3: B — High recall means retrieval is working; low faithfulness means the generator is going off-script
