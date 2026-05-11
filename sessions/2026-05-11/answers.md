---
date: 2026-05-11
topic: Agent Development
score: pending
---

# Answers — 2026-05-11
## Topic: Agent Development

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
Q1: B — The description is the model's only signal for when and how to call a tool; it directly controls invocation decisions, not validation or auth.
Q2: B — The tool result must be returned as a tool-role message with the tool name and return value; the model needs this message type specifically to continue the conversation correctly.
Q3: C — Exponential backoff with jitter spreads retry load and respects rate limit windows; fixed-delay retries hit the same rate limit wall repeatedly.
Q4: C — A handoff targets one specific agent and transfers context and control; a broadcast sends a message to all agents without a specific recipient or control transfer.
Q5: B — Anchoring bias: when the Evaluator sees prior agents' reasoning and justifications, it anchors to their conclusions. Fix is to scope handoff context — give the Evaluator only the original task and final output.
Q6: B — Backpressure handling with an async queue paces token delivery to match consumer capacity; temperature and chunk size don't control delivery rate relative to consumer speed.
Q7: C — The tool description is what the model uses to decide when to invoke a tool; a precise, scoped description is the most direct and reliable control mechanism.
Q8: B — Checkpoints serialize workflow state at each stage to a durable store, enabling resumption from the last checkpoint without reprocessing completed stages.
Q9: B — Non-zero temperature introduces sampling variance; identical inputs with the same tool schemas can still produce different tool selections across runs when temperature > 0.
Q10: C — A parallel evaluation harness with a judge model scales to 200 cases in minutes, produces consistent scores, and integrates into CI for automated gating on every prompt change.
