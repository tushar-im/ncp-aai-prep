---
date: 2026-05-12
topic: Knowledge Integration and Data Handling + Safety, Ethics, and Compliance
sections:
  - Knowledge Integration and Data Handling
  - Safety, Ethics, and Compliance
weight: 15
phase: 1
day: 7
days_until_exam: 54
status: pending
---

# Study Session — 2026-05-12
## Topic: Knowledge Integration and Data Handling + Safety, Ethics, and Compliance
## Exam Sections: Knowledge Integration and Data Handling (10%) + Safety, Ethics, and Compliance (5%)
## Estimated Time: 1 hour

---

## Concept Brief

### Knowledge Integration and Data Handling

RAG (Retrieval-Augmented Generation) is the dominant pattern for grounding agent responses in external knowledge without retraining the model. Your RAG pipeline on the oncology EMR is the canonical example — patient documents are chunked, embedded, stored in a vector store, and retrieved at query time to give the model relevant clinical context.

**Chunking strategy matters more than most people realize.** Fixed-size chunking (e.g. 512 tokens with 50-token overlap) is simple but breaks semantic units. Semantic chunking splits on paragraph or section boundaries. Hierarchical chunking stores both summary chunks and fine-grained chunks — the retriever fetches the summary first, then drills in. For clinical notes, where a sentence mid-paragraph can change the meaning of everything around it, semantic or hierarchical chunking outperforms fixed-size.

**Embedding models determine retrieval quality.** Dense embeddings (e.g. `text-embedding-ada-002`, NVIDIA `nv-embed` models) map text to a vector space where semantic similarity equals cosine proximity. Sparse embeddings (BM25) capture keyword overlap. Hybrid search combines both — BM25 for precision on medical codes, dense for semantic recall on clinical concepts. Your RAG pipeline almost certainly does this hybrid pattern even if you don't explicitly call it that.

**Vector databases** (Chroma, Weaviate, pgvector, Pinecone, Milvus) handle ANN (approximate nearest neighbor) search at scale. HNSW (Hierarchical Navigable Small World) is the dominant indexing algorithm — logarithmic search time, tunable precision via `ef` and `M` parameters. For exam purposes: know that higher `M` = better recall, higher memory cost.

**Knowledge graphs** complement RAG by encoding explicit relationships between entities. Your CodeIndex is basically a code knowledge graph — ast-grep extracts structural relationships (function calls, class inheritance, import chains) into SQLite. In agent systems, graphs let you answer "what depends on what" queries that a vector search can't handle because vector search finds semantically similar text, not structured traversal paths. The NCP-AAI exam treats knowledge graphs as a distinct retrieval mode alongside RAG.

**Context management** is about what you stuff into the model's context window. Naive RAG dumps all retrieved chunks; smarter approaches use re-ranking (cross-encoder models score chunk relevance after initial retrieval), maximal marginal relevance (MMR — pick diverse chunks, not just top-k similar), and context compression (LLMLingua, prompt compression). In multi-agent systems, context grows across turns — you need explicit truncation or summarization strategies to prevent context bleed.

**Structured vs. unstructured data handling.** NLFHIR is your example here — natural language → FHIR R4 means translating unstructured text into a typed, relational schema. Agents that handle structured data need schema awareness, validation, and graceful degradation when the schema doesn't match expectations.

---

### Safety, Ethics, and Compliance

**Responsible AI** in the NCP-AAI context covers four pillars: fairness (outputs don't systematically disadvantage groups), transparency (model can explain or trace its reasoning), privacy (PII/PHI handled correctly), and accountability (human oversight preserved).

**Bias and fairness.** Bias enters at three stages: data (training data reflects historical biases), model (model amplifies certain patterns), and pipeline (retrieval or ranking steps filter out certain populations' data). Detection requires disaggregated evaluation — measuring performance separately across demographic groups. Mitigation options: re-sampling training data, adversarial debiasing, constrained optimization.

**Privacy-preserving techniques.** Your de-id.org work is the direct exam anchor here. PHI de-identification under HIPAA Safe Harbor = 18 identifiers removed (name, DOB, phone, MRN, address, etc.). Programmatic approaches: NER-based (Stanford NER, spaCy) to tag entities, then mask or replace. Synthetic data generation (replace with realistic fake values) preserves statistical distributions for training. Differential privacy adds calibrated noise to outputs so individual records can't be inferred.

**Guardrails** are the runtime safety layer. NeMo Guardrails (NVIDIA's framework) uses a declarative Colang language to define topical rails (what the agent can/can't discuss), output rails (format and content checks), and dialog flow constraints. At the code level, guardrails are typically implemented as pre/post-processing hooks on model inputs/outputs. Your PACE loop's Evaluator is a manual guardrail — it checks compliance artifact quality before output is accepted.

**Compliance frameworks** relevant to your work: HIPAA (PHI handling, minimum necessary standard, audit logging), SOC2 (availability, confidentiality, processing integrity), PCI-DSS (payment card data, less relevant to healthcare but PACE scans for it). In agent systems, compliance manifests as: audit trails (every agent action logged with inputs/outputs), role-based access control on tool calls, and data residency constraints.

**Human-in-the-loop (HITL) for safety.** High-stakes agent actions (deleting records, sending external communications, submitting claims) should require human confirmation. This is distinct from evaluation HITL (where a human grades agent outputs to improve the model) — safety HITL is about preventing irreversible actions, not about feedback collection.

**Content safety** covers output filtering for harmful, biased, or inappropriate content. In clinical AI, this also covers hallucination — the model asserting a clinical fact not in the retrieved context. Faithfulness metrics (RAGAs faithfulness score, your study-brain has this) measure how well model outputs are grounded in retrieved context.

---

## Sub-topics Covered

- **6.1 RAG architectures** — STRONG (built oncology RAG pipeline)
- **6.2 Vector databases and embeddings** — STRONG (used in production)
- **6.3 Chunking and indexing strategies** — REVIEW (used it, haven't named the patterns formally)
- **6.4 Knowledge graphs in agent systems** — STRONG (CodeIndex is a code knowledge graph)
- **6.5 Context management and grounding** — REVIEW (know the problem, less clear on named techniques like MMR)
- **9.1 Responsible AI principles** — REVIEW (know it philosophically, not exam-vocabulary level)
- **9.2 Bias, fairness, and model transparency** — REVIEW (know the concepts, weaker on specific techniques)
- **9.3 Data privacy and regulatory compliance** — STRONG (de-id.org, HIPAA work)
- **9.4 Content safety and guardrails** — REVIEW (know guardrails conceptually, NeMo specifics are NEW)
- **9.5 Human oversight and accountability** — REVIEW (HITL in PACE is implicit, not formally named)

---

## Key Points to Remember

- RAG retrieval quality is determined by: chunking strategy + embedding model + retrieval method (dense/sparse/hybrid) + re-ranking. Changing any one of these changes quality.
- HNSW is the dominant ANN indexing algorithm. Higher `M` = better recall, more memory. This will appear on the exam.
- Knowledge graphs answer structural/relational queries that vector search cannot. Vector search finds *similar text*; graphs find *connected entities*.
- HIPAA Safe Harbor de-identification requires removing 18 specific identifier types — not just "names and DOBs."
- NeMo Guardrails uses Colang (declarative DSL) for defining rails. This is NVIDIA-specific and exam-testable.
- Faithfulness (is the output grounded in retrieved context?) is separate from answer relevance (does the answer address the question?). Both are RAG evaluation metrics.
- Safety HITL (blocking irreversible actions) is conceptually distinct from evaluation HITL (collecting human feedback for training).

---

## Go Deeper

- `study-brain/ragas/faithfulness.md` — read the faithfulness metric definition and how it's computed; this directly connects RAG grounding to safety (hallucination detection)
- `study-brain/ragas/context_precision.md` and `context_recall.md` — understand the difference between these two; context precision is about retrieval noise, context recall is about retrieval completeness
- `study-brain/ragas/nvidia_metrics.md` — check if NVIDIA's custom metrics differ from base RAGAs; exam may test NVIDIA-specific tooling

---

## Cheatsheet

**RAG** — Retrieval-Augmented Generation; grounds LLM outputs in external documents retrieved at inference time, no retraining needed

**Chunking (semantic)** — splitting documents at semantic boundaries (paragraphs, sections) rather than fixed token counts; better for preserving context

**HNSW** — Hierarchical Navigable Small World; ANN indexing algorithm used in most vector databases; `M` controls graph connectivity (recall vs. memory tradeoff)

**Hybrid search** — combining dense (embedding) + sparse (BM25/keyword) retrieval; dense = semantic similarity, sparse = exact keyword match; combined = better precision + recall

**MMR (Maximal Marginal Relevance)** — re-ranking strategy that selects diverse chunks rather than top-k most-similar; prevents redundant context stuffing

**Cross-encoder re-ranking** — second-stage relevance scoring using a model that reads query + chunk together; more accurate than cosine similarity but slower

**HIPAA Safe Harbor** — de-identification standard requiring removal of 18 specific PHI identifiers; the legal baseline for sharing health data without patient authorization

**NeMo Guardrails** — NVIDIA's framework for declarative safety rails on LLM apps; uses Colang DSL; supports topical, output, and dialog-flow constraints

**Faithfulness (RAG)** — measures whether model claims are supported by retrieved context; high faithfulness = low hallucination rate

**Differential privacy** — adds calibrated mathematical noise to model outputs/training so individual records cannot be reverse-engineered from aggregate results

---

## Exam Questions

Q1: In a RAG pipeline for clinical notes, which chunking strategy best preserves the semantic integrity of patient encounter summaries?
A) Fixed-size chunking at 256 tokens with no overlap
B) Fixed-size chunking at 512 tokens with 50-token overlap
C) Semantic chunking at paragraph and section boundaries
D) Character-level chunking at 1000 characters

Q2: Your vector database uses HNSW indexing. You need to improve recall at the cost of higher memory usage. Which parameter do you increase?
A) `ef_search`
B) `M`
C) `nprobe`
D) `num_leaves`

Q3: A medical agent retrieves the top-5 most similar chunks for every query. All 5 chunks often say the same thing in slightly different words. What retrieval technique addresses this?
A) Cross-encoder re-ranking
B) Maximal Marginal Relevance (MMR)
C) BM25 sparse retrieval
D) Hierarchical chunking

Q4: Under HIPAA Safe Harbor, which of the following is NOT one of the 18 identifiers that must be removed for de-identification?
A) Medical record number
B) Patient's clinical diagnosis
C) Device identifiers and serial numbers
D) Certificate and license numbers

Q5: A developer needs NLP-based PHI detection before data reaches a vector store. Which approach correctly combines detection and replacement?
A) Run BM25 search to find text with high keyword frequency, then mask matches
B) Use NER to tag PHI entity spans, then replace tagged spans with synthetic or redacted values
C) Use cosine similarity to find documents similar to known PHI examples, then delete them
D) Apply differential privacy noise to all text before indexing

Q6: NeMo Guardrails is configured to prevent the agent from discussing competitor products. In which rail category does this constraint belong?
A) Output rail
B) Dialog flow rail
C) Topical rail
D) Input validation rail

Q7: An agent in a compliance pipeline produces an output that is grammatically correct and answers the user's question, but cites a drug dosage not found in any retrieved document. Which RAG evaluation metric would flag this?
A) Answer relevance
B) Context recall
C) Faithfulness
D) Context precision

Q8: A healthcare AI team needs to share patient data with a research institution for model training without violating HIPAA. The data includes rare disease patients where removing the 18 Safe Harbor identifiers may still leave individuals re-identifiable. What additional technique should they apply?
A) Encrypt the dataset with AES-256 before transfer
B) Apply differential privacy to add calibrated noise to the dataset
C) Switch to hybrid retrieval to reduce the number of records needed
D) Use NeMo Guardrails to filter outputs at inference time

Q9: Your RAG pipeline is returning relevant documents but the model's answers are still hallucinating facts not in those documents. Retrieval metrics look good (context precision 0.9, context recall 0.85). What is the most likely failure point?
A) The embedding model is producing poor-quality vectors
B) Chunking is splitting documents at wrong boundaries
C) The generation model is ignoring retrieved context and drawing on parametric memory
D) HNSW index parameters are misconfigured

Q10: A PACE-like compliance pipeline routes failed evaluations back to the Coder agent for revision. A new regulation requires that any agent action that modifies a production record must be approved by a human before execution. What is the correct architectural change?
A) Add a NeMo Guardrails topical rail to block production record edits
B) Increase the Evaluator's scoring threshold so fewer outputs pass
C) Insert a human-in-the-loop checkpoint between the Evaluator approval and the production write action
D) Add a second Evaluator agent to cross-check decisions before they're executed
