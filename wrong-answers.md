# Wrong Answers Log

Automatically updated during quiz and mock exam sessions.
Review before exam week.

## Format
### [Date] — [Topic]
**Q**: [Question]
**My answer**: [What I said]
**Correct**: [Correct answer]
**Why**: [Explanation]

---

<!-- Entries added here automatically during sessions -->

### 2026-05-08 — Agent Architecture and Design

**Q**: An orchestrator passes the full reasoning chain of every previous agent to each subsequent agent. The Evaluator consistently scores outputs higher than expected. What is the most likely architectural cause?
**My answer**: Orchestrator-Subagent pattern
**Correct**: Anchoring bias from context bleed
**Why**: When an evaluator sees prior agents' reasoning and justifications, it anchors to their conclusions and scores leniently. Fix is context scoping — give the evaluator only the original request + final output, not the reasoning chain.

**Q**: A compliance pipeline (PACE-like) with four agents where the Evaluator re-routes failed tasks back to the Coder. Best architectural description?
**My answer**: D — Blackboard architecture with passive agents
**Correct**: B — Sequential pipeline with a feedback loop
**Why**: Blackboard = shared passive data store all agents read/write. PACE is a fixed-order pipeline (P→A→C→E) with a feedback loop, not a shared store. Don't confuse orchestrator-worker or pipeline+feedback with blackboard.
