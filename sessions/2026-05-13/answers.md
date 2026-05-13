---
date: 2026-05-13
topic: Knowledge Integration and Data Handling + Safety, Ethics, and Compliance
score: pending
---

# Answers — 2026-05-13
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
Q1: B — Advanced RAG specifically adds query rewriting (pre-retrieval) and post-retrieval re-ranking on top of Naive RAG's basic pipeline.
Q2: B — RRF formula is 1/(k + rank); it merges ranked lists from multiple retrieval sources without needing score normalization.
Q3: C — Anonymization is irreversible. Pseudonymization uses a key and is reversible. HIPAA Safe Harbor requires anonymization (irreversible removal of 18 identifiers).
Q4: C — Input rails screen user messages before they reach the LLM. Output rails validate LLM responses. Dialog rails manage flow.
Q5: C — Cross-encoder re-ranking then truncation is the correct approach: most relevant chunks are preserved, context is minimal. MapReduce and Refine increase LLM calls, not reduce context.
Q6: B — Entities + typed relationships + traversal = knowledge graph / structured knowledge base. Not episodic (no time-sequenced events), not a vector index.
Q7: C — Automation bias: the clinician stopped critically evaluating because the AI output said "high-confidence." This is the core risk in clinical AI deployment.
Q8: B — Sending un-de-identified PHI to a third-party LLM provider violates transmission security requirements and requires a signed BAA. Minimum Necessary is secondary.
Q9: A — High recall, low precision: retrieved everything relevant but also pulled in irrelevant chunks. This adds noise and increases the chance the LLM hallucinates from off-topic context.
Q10: B — A is bi-encoder (query and doc encoded independently, cosine similarity — fast, used at retrieval scale). B is cross-encoder (concatenated, single forward pass — slower, more accurate, used for re-ranking top-K).
