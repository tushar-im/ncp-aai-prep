---
date: 2026-05-13
topic: Knowledge Integration and Data Handling + Safety, Ethics, and Compliance
sections:
  - Knowledge Integration and Data Handling
  - Safety, Ethics, and Compliance
weight: 15
phase: 1
day: 6
days_until_exam: 53
status: pending
---

# Study Session — 2026-05-13
## Topic: Knowledge Integration and Data Handling + Safety, Ethics, and Compliance
## Exam Section: Knowledge Integration (10%) + Safety/Ethics/Compliance (5%)
## Estimated Time: 1 hour

---

## Concept Brief

You've built the RAG pipeline and de-id.org. This session maps what you already know to exam vocabulary and closes any labeling gaps.

**Knowledge Integration and Data Handling**

RAG architecture is the exam's primary vehicle for testing this section. The canonical taxonomy has three tiers: Naive RAG (chunk → embed → retrieve → generate), Advanced RAG (adds pre-retrieval query rewriting, post-retrieval re-ranking, and hybrid search), and Modular RAG (swappable retrieval modules, routing logic, multi-index architectures). Your oncology EMR pipeline is Modular RAG — vector search on clinical notes, structured FHIR queries from NLFHIR, patient document retrieval. Know which tier a described system falls into.

Chunking strategy is consistently tested. Fixed-size chunking (e.g., 512 tokens with 10% overlap) is simple but splits semantic units. Sentence-window chunking stores expanded context around a retrieved sentence, returning more coherent passages. Hierarchical chunking (document → section → chunk) supports parent-child retrieval — retrieve the chunk, return the section. Semantic chunking uses embedding similarity to detect natural breaks. Your CodeIndex uses structural chunking (AST node boundaries), which is the code-domain equivalent of semantic chunking.

Vector database concepts that appear on the exam: approximate nearest neighbor (ANN) algorithms (HNSW, IVF-Flat, IVF-PQ), the distance metric tradeoff (cosine similarity for normalized embeddings, dot product for unnormalized, Euclidean for spatial data), and index types. Hybrid search = dense retrieval (embeddings) + sparse retrieval (BM25/TF-IDF) combined with Reciprocal Rank Fusion (RRF) or learned rerankers (cross-encoders).

Context window management: stuffing (throw everything in), mapreduce (chunk-level LLM calls then aggregate), refine (iterative document chaining), and rerank-then-truncate. The exam tests when each is appropriate. For long-context agents with many retrieved chunks, rerank-then-truncate is most token-efficient.

Knowledge graphs store entities and typed relationships (triples: subject–predicate–object). In agentic systems, they serve as long-term structured memory. Your CodeIndex is effectively a code knowledge graph — functions, files, imports, call edges, stored in SQLite with ast-grep for traversal. The exam may call this a "structured knowledge base" or "semantic knowledge store."

Grounding strategies reduce hallucination: citation-based grounding (cite the retrieved chunk), self-consistency checking (multiple generations voted), retrieval-augmented verification, and Constitutional AI constraints.

**Safety, Ethics, and Compliance**

Bias types the exam tests: selection bias (training data distribution mismatch), confirmation bias (agent preferentially retrieves confirming evidence), automation bias (human over-trusts agent output), and representational bias (demographic skew in outputs). Your PACE evaluator is specifically designed to catch confirmation bias in LLM-generated compliance summaries.

PHI/PII handling: de-identification methods are tokenization, pseudonymization (reversible), anonymization (irreversible), and differential privacy (adds calibrated noise). Your de-id.org uses NER-based named entity recognition (Stanford NER) + OCR (PaddleOCR) for document-level PHI detection — this maps to the "automated de-identification pipeline" pattern the exam tests.

Regulatory frameworks: HIPAA technical safeguards require access control, audit logging, transmission security, and integrity controls. SOC2 Type II attests to operational controls over a period. PCI-DSS governs cardholder data — scope minimization (don't touch what you don't need) is the key principle. Your PACE pipeline scans for violations across all three — the exam may give a scenario and ask which framework applies.

NeMo Guardrails is NVIDIA's primary safety layer for agents — it implements input/output rails via Colang configuration. Input rails screen user messages (topical, jailbreak, moderation). Output rails validate responses before delivery. Dialog rails manage conversation flow. Know these three types.

Audit trails for AI systems require: request ID linkage, input/output logging, model version tracking, and human-review flags. These map directly to compliance in healthcare AI (your Bedrock setup).

---

## Sub-topics Covered

- **6.1 RAG pipeline design and optimization** — STRONG (built oncology EMR RAG)
- **6.2 Vector database selection and embedding strategies** — STRONG (production vector search)
- **6.3 Chunking strategies and document preprocessing** — REVIEW (built it, but exam vocab needs reinforcement)
- **6.4 Context window management and retrieval patterns** — REVIEW (know it in practice, need exam naming)
- **6.5 Knowledge graphs as structured agent memory** — STRONG (CodeIndex is a code knowledge graph)
- **6.6 Hybrid search and re-ranking** — REVIEW (know BM25 + dense, exam specifics on RRF need checking)
- **9.1 Bias detection and types in agent systems** — REVIEW (PACE catches confirmation bias, full taxonomy needs study)
- **9.2 PHI/PII de-identification methods** — STRONG (de-id.org, healthcare compliance background)
- **9.3 Regulatory compliance (HIPAA, SOC2, PCI-DSS)** — STRONG (PACE explicitly covers these)
- **9.4 NVIDIA NeMo Guardrails** — NEW (know the concept, not the config/Colang specifics)
- **9.5 Audit trails and AI governance** — REVIEW (built logging, needs exam framing)

---

## Key Points to Remember

- **Modular RAG** = swappable retrieval modules + routing. Naive RAG has no query rewriting or re-ranking. Know the three tiers.
- **RRF (Reciprocal Rank Fusion)** = standard algorithm for merging dense + sparse retrieval scores in hybrid search. Score = Σ 1/(k + rank_i).
- **HNSW** = Hierarchical Navigable Small World — the default ANN algorithm for high-dimensional vector search in most production vector DBs (Pinecone, Weaviate, Qdrant).
- **NeMo Guardrails** has three rail types: **input rails** (screen incoming messages), **output rails** (validate responses), **dialog rails** (control conversation flow). Configured in Colang.
- **Pseudonymization** is reversible (key-based token swap). **Anonymization** is irreversible. Exam distinguishes them — HIPAA Safe Harbor uses anonymization.
- **Cross-encoders** re-rank retrieved chunks using full query-document attention (slower, more accurate). **Bi-encoders** generate independent embeddings (faster, used at retrieval time).
- **Automation bias** = human over-trusts AI output and stops critically evaluating. Most relevant risk in clinical AI systems like your Bedrock assistant.

---

## Go Deeper

- `study-brain/ragas/context_precision.md` — Context Precision metric: measures whether retrieved chunks are relevant. Read the scoring formula and the distinction between context precision and context recall.
- `study-brain/ragas/faithfulness.md` — Faithfulness metric: measures whether the generated answer is grounded in retrieved context. Key for exam questions on hallucination reduction.
- `study-brain/ragas/context_recall.md` — Context Recall: measures whether all ground truth information was retrieved. Understand the difference from precision — this is a classic exam trap.

---

## Cheatsheet

**Naive RAG** — basic chunk→embed→retrieve→generate with no query rewriting or post-retrieval filtering

**Advanced RAG** — adds query rewriting (pre-retrieval) and re-ranking/filtering (post-retrieval) to Naive RAG

**Modular RAG** — fully swappable retrieval modules, routing logic, multi-index support; most production-grade systems

**HNSW** — Hierarchical Navigable Small World; default ANN algorithm for approximate vector search in production vector DBs

**RRF** — Reciprocal Rank Fusion; merges dense + sparse retrieval rankings: score = Σ 1/(k + rank_i), k typically 60

**Cross-encoder** — re-ranker that scores query-document pairs with full attention; slower but more accurate than bi-encoder at ranking time

**Colang** — NVIDIA's configuration language for NeMo Guardrails; defines input, output, and dialog rails

**Pseudonymization** — reversible PHI replacement using a key (HIPAA-compliant with safeguards); anonymization is irreversible

**Automation bias** — human tendency to over-trust AI output and reduce critical evaluation; primary risk in clinical AI deployment

**Context Precision (RAG metric)** — fraction of retrieved chunks that are actually relevant to the query; high precision = low noise in context

---

## Exam Questions

Q1: Which RAG architecture tier adds query rewriting before retrieval and cross-encoder re-ranking after retrieval?
A) Naive RAG
B) Advanced RAG
C) Modular RAG
D) Retrieval-Augmented Verification

Q2: An agent system uses BM25 for lexical retrieval and a bi-encoder for semantic retrieval, then merges their ranked lists using 1/(k + rank). Which algorithm is being applied?
A) Maximum Marginal Relevance (MMR)
B) Reciprocal Rank Fusion (RRF)
C) Hierarchical Navigable Small World (HNSW)
D) Inverted File Index (IVF)

Q3: You need to de-identify a document for research sharing, and the substitution must be irreversible so the original identifiers cannot be recovered. Which method is correct?
A) Tokenization with a secure vault
B) Pseudonymization
C) Anonymization
D) Differential privacy with epsilon = 0.1

Q4: NeMo Guardrails uses Colang to define three types of rails. Which rail type is responsible for blocking jailbreak attempts in the user's message before it reaches the LLM?
A) Output rail
B) Dialog rail
C) Input rail
D) Topical constraint

Q5: Your oncology RAG pipeline returns 20 chunks but the LLM context window only fits 5. Reducing context size while preserving the most relevant chunks is the goal. Which approach is most appropriate?
A) MapReduce — run LLM over each chunk separately and aggregate
B) Stuff all 20 chunks and rely on LLM attention to focus
C) Re-rank the 20 chunks with a cross-encoder, then truncate to top 5
D) Refine — chain chunks sequentially, updating the answer iteratively

Q6: A CodeIndex system stores functions, files, imports, and call-graph edges in SQLite and uses ast-grep for traversal. In agent architecture terminology, this is best described as:
A) Episodic memory store
B) Structured knowledge base / code knowledge graph
C) Semantic cache
D) Vector index with BM25 fallback

Q7: A clinical AI assistant flags an output as high-confidence. A clinician reviews it and approves it without reading the supporting citations. This is an example of which safety risk?
A) Confirmation bias
B) Selection bias
C) Automation bias
D) Representational bias

Q8: You are building a HIPAA-compliant RAG pipeline. Retrieved patient chunks are passed to the LLM without de-identification. The LLM is hosted by a third-party cloud provider. Which HIPAA requirement is most directly at risk?
A) Minimum Necessary standard
B) Transmission security (encryption in transit) and Business Associate Agreement
C) Safe Harbor de-identification
D) Access control and audit logging

Q9: A RAG system has high context recall but low context precision. What does this mean, and what is the likely user-facing problem?
A) The system retrieves all relevant information but also retrieves a lot of irrelevant chunks, increasing noise and hallucination risk
B) The system retrieves very relevant chunks but misses some ground truth information, leading to incomplete answers
C) The system generates faithful answers but they don't match the retrieved context
D) The embedding model is underfit — increase vector dimensions

Q10: You are comparing two re-ranking approaches for a retrieval system. Approach A encodes the query and all documents independently and uses cosine similarity. Approach B concatenates query and document and scores them with a single transformer forward pass. Which is which, and what is the tradeoff?
A) A is a cross-encoder (accurate, slow); B is a bi-encoder (fast, less accurate at ranking)
B) A is a bi-encoder (fast, independent encoding); B is a cross-encoder (accurate, slower); use A at retrieval scale, B for final re-ranking
C) Both are bi-encoders; B just uses a larger model
D) A uses HNSW; B uses IVF-PQ; the tradeoff is index build time vs query latency
