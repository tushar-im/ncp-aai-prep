# Session Notes — 2026-05-08

## Topic
Agent Architecture and Design (15% exam weight)
Phase 1, Day 3 — Deep read + concept mapping to PACE/CodeIndex/RAG

## Key Concepts Covered

### 1. Perceive-Plan-Act-Reflect Loop
The foundational agent loop. Every architecture maps to these four stages. PACE implements all four explicitly.

### 2. Agent Types
- Reactive: no memory, pure stimulus-response
- Deliberative: maintains state, plans ahead (PACE's Planner)
- Hybrid: reactive layer for speed + deliberative for complex tasks (most production agents)

### 3. Multi-Agent Patterns
- Orchestrator-worker: centralized planner, specialized workers (PACE)
- Pipeline: linear A→B→C (RAG pipeline)
- Peer-to-peer: direct agent communication, no central controller
- Blackboard: shared passive data store all agents read/write (NOT PACE)

### 4. Context Scoping (deep dive)
- Developer-controlled decision: what each agent sees at each hop
- Prevents anchoring bias in evaluator agents
- Quality of context > quantity of context
- Stateless scoping: orchestrator decides at call time
- Stateful scoping: agent's retrieval logic decides
- PACE implementation: each agent gets minimal, purposeful context — Evaluator sees original request + Coder output only

### 5. Stateful vs Stateless Agents
- Stateless: state in the message, deterministic, easy to scale and debug
- Stateful: state in external store, flexible but non-deterministic retrieval
- Migration risk: stateless → stateful can cause inconsistency from non-deterministic retrieval

### 6. ReAct Framework (brief intro)
- Reasoning + Acting interleaved
- Thought (scratchpad, no side effects) → Action (tool call) → Observation (result injected into context)
- Termination condition required
- Failure modes: anchoring bias, observation poisoning, premature termination

### 7. Tool Schemas
- Description field is what the LLM uses to decide when/how to call a tool
- Bad descriptions = wrong tool calls, not bad execution

## Official Exam Questions (from session-study.md)

| Q | Tushar's Answer | Correct | Result |
|---|----------------|---------|--------|
| Q1: Agent loop stage for deciding next action | C — Planning | C | Correct |
| Q2: No memory + horizontal scaling describes | D — Stateless agent | D | Correct |
| Q3: Who decomposes tasks in orchestrator-worker | B — Central orchestrator | B | Correct |
| Q4: Primary purpose of tool schema | B — Name/description/input format for LLM | B | Correct |
| Q5: PACE-like pipeline with Evaluator re-routing | D — Blackboard | B — Sequential pipeline with feedback loop | Wrong |

**Score: 4/5**

## Improvised Questions (mid-session)

| Q | Tushar's Answer | Correct | Result |
|---|----------------|---------|--------|
| Why does Evaluator over-score when given full reasoning chain? | Orchestrator-Subagent | Anchoring bias from context bleed | Wrong |
| Best scoping strategy for independent Auditor | B — Original request + Reporter output only | B | Correct |
| Stateful migration causes inconsistency — why? | Retrieval losing data | Non-deterministic retrieval (different chunks per run) | Partial |

## Concepts to Revisit
- **Anchoring bias** — know this term cold. When an evaluator sees prior reasoning chains, it anchors to them and scores leniently. Fix = context scoping.
- **Blackboard pattern** — agents communicate through a shared passive store, not directly. Don't confuse with orchestrator-worker.

## Vocabulary Gaps
- Knows the concepts, misses the exact exam terms (anchoring bias, blackboard)
