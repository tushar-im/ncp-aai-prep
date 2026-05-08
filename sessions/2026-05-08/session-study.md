---
# Study Session — 2026-05-08
## Topic: Agent Architecture and Development
## Exam Section: Agent Architecture and Design (15%) + Agent Development (15%)
## Estimated Time: 1 hour

## Concept Brief

Agent architecture defines how an AI system reasons, acts, and loops. Two exam sections share this territory — Architecture covers structure and design patterns, Development covers implementation mechanics.

**The agent loop** is the foundation. At minimum: perceive input → reason → select action → execute tool → observe result → repeat. PACE runs this loop explicitly: Planner decomposes the task, Author drafts, Coder implements, Evaluator checks output and routes back if it fails. That's a 4-node DAG with a conditional feedback edge — exactly what exam questions probe.

**Orchestration patterns** you need to know:
- *Sequential*: agents hand off one to the next (your PACE chain)
- *Parallel*: multiple agents run concurrently, results merged
- *Router/Selector*: a controller agent dispatches to specialists based on input class
- *Hierarchical*: planner agents spawn sub-agents, results bubble up

**Tool use** is a first-class concept. Agents call tools (functions, APIs, retrieval systems) and get back structured results. CodeIndex is a tool layer — your MCP interface gives agents structured access to a code knowledge graph. The exam tests whether you know the difference between tool selection (which tool fits the query?) and tool invocation (calling it correctly with right parameters).

**State and memory** across turns: agents need to pass context. Short-term context lives in the conversation window. Long-term state requires external storage. PACE has implicit state — each agent's output is the next agent's input, which is the simplest stateful pattern.

**Error handling in agent development**: tools fail, LLMs hallucinate tool calls, loops can spin. Robust agents need retry logic, fallback paths, and loop-break conditions. Your PACE Evaluator is the loop-break: if compliance check fails N times, escalate rather than loop forever.

**Multi-agent coordination** requires a shared message bus or shared state store. Agents need to know what other agents are doing without tight coupling. This is where orchestration frameworks (LangGraph, CrewAI, NVIDIA Agent Toolkit) add value.

## Key Points to Remember

- Agent loop = perceive → reason → act → observe. Know all four stages by name.
- Orchestration patterns: sequential, parallel, router, hierarchical — know when each applies.
- Tool use: agents don't execute code directly, they call tools and interpret results.
- Multi-agent systems need a coordinator/orchestrator that owns task decomposition and result aggregation.
- Loop termination conditions are an explicit design decision — always deliberate, never implicit.

## Study Brain References

No study-brain files directly cover this topic yet. The nvidia-study-guide.md needs to be added (see study-brain/README.md). Today's Phase 1 goal is to read the study guide sections on Agent Architecture and Development and tag sub-topics GREEN/YELLOW/RED.

## Exam Questions

Q1: In a multi-agent system, which orchestration pattern is most appropriate when different input types require completely different processing pipelines?
A) Sequential chaining — each agent processes every request in order
B) Router/selector — a controller dispatches to specialized agents based on input class
C) Parallel execution — all agents process every request simultaneously
D) Hierarchical decomposition — a planner recursively breaks tasks into subtasks

Q2: An agent is calling a retrieval tool that intermittently returns empty results. The agent is stuck in a loop, repeatedly calling the same tool with the same query. Which design principle was violated?
A) Tool selection logic — the agent picked the wrong tool
B) Memory isolation — the agent doesn't have access to prior tool results
C) Loop termination conditions — the agent lacks a break condition for repeated failures
D) Parallel orchestration — the agent should call multiple tools simultaneously

Q3: In NVIDIA's agentic AI framework, what is the primary role of an orchestrator agent?
A) Execute individual tool calls and return raw results to the user
B) Store long-term memory and retrieve relevant context for sub-agents
C) Decompose high-level goals into subtasks and coordinate specialist agents to complete them
D) Evaluate final output quality and apply guardrails before returning to the user

## Answers
Q1: B — Router/selector is the canonical pattern for input-type-based dispatch to specialized pipelines.
Q2: C — Missing loop termination is the specific failure mode; the agent needs a max-retry or fallback condition.
Q3: C — Orchestrators own decomposition and coordination; evaluation/guardrails is a separate concern.
---
