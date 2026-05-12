---
date: 2026-05-12
topic: Knowledge Integration and Data Handling + Safety, Ethics, and Compliance
score: pending
---

# Answers — 2026-05-12
## Topic: Knowledge Integration and Data Handling + Safety, Ethics, and Compliance

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
Q1: C — Semantic chunking at paragraph/section boundaries preserves the natural structure of clinical encounter notes; fixed-size chunking breaks mid-thought and degrades retrieval quality.
Q2: B — `M` controls the number of bidirectional links per node in the HNSW graph; higher M = better recall at the cost of memory. `ef_search` controls query-time beam width (also affects recall but is a search param, not index param).
Q3: B — MMR (Maximal Marginal Relevance) penalizes chunks that are too similar to already-selected chunks, promoting diversity in the retrieved context set.
Q4: B — Clinical diagnosis is NOT one of the 18 HIPAA Safe Harbor identifiers. The 18 are administrative/demographic/device identifiers (names, dates, geo, phone, MRN, device IDs, etc.) — not clinical content.
Q5: B — NER-based tagging (e.g. Stanford NER, spaCy) identifies PHI entity spans by type, then replacement (masking or synthetic substitution) handles the actual de-identification. BM25 and cosine similarity are retrieval tools, not NER.
Q6: C — Topical rails restrict what subjects the agent is allowed to discuss. Output rails check the content/format of generated responses. Dialog flow rails constrain conversation sequencing.
Q7: C — Faithfulness measures whether the model's claims are supported by retrieved context. A hallucinated drug dosage not found in context = faithfulness failure. Answer relevance measures if the answer addresses the question (which it does here).
Q8: B — Differential privacy adds mathematically calibrated noise so individual records cannot be re-identified even when de-identification alone is insufficient. Encryption protects data in transit but doesn't prevent re-identification. Guardrails operate at inference, not on stored data.
Q9: C — If retrieval metrics are strong (you're getting the right documents) but answers still hallucinate, the failure is in the generation step — the model is relying on parametric (training) memory rather than the provided context. Fix: strengthen prompting to explicitly instruct the model to only use retrieved context, or use a smaller/less "opinionated" model.
Q10: C — A human-in-the-loop checkpoint between approval and execution is the correct safety HITL pattern for irreversible or high-stakes actions. Guardrails (A) are for content/topic restrictions, not action authorization. A stricter evaluator (B) doesn't add human oversight. A second evaluator (D) is still automated — not HITL.
