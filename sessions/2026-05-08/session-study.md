---
date: 2026-05-08
topic: Agent Architecture and Design
sections:
  - Agent Architecture and Design
  - Agent Development
weight: 30%
phase: 1
day: 3
days_until_exam: 58
status: pending
---

# Study Session — 2026-05-08
## Topic: Agent Architecture and Design
## Exam Section: Agent Architecture and Design (15% of exam)
## Estimated Time: 1 hour

---

## Concept Brief

An AI agent is not just an LLM with a prompt. It's a system with a loop: perceive inputs, plan a response, execute actions via tools, and reflect on what happened. This perceive-plan-act-reflect cycle is the foundational architecture for every agentic system, and the exam tests whether you understand each stage and how design choices affect the whole.

**The Core Agent Loop**

Perception: the agent receives inputs — a user query, tool output, memory retrieval result, sensor data. For your RAG pipeline, perception is the retrieval step: the agent sees a patient query and the top-k chunks returned from your vector store. For PACE, each agent perceives the previous agent's output as its input.

Planning: the agent decides what to do next. This can be as simple as a single LLM call ("given this context, generate an answer") or as complex as a multi-step decomposed plan. PACE's Planner agent does this explicitly — it breaks a compliance scan request into subtasks and routes them.

Action: the agent executes — calls a tool, writes to memory, sends a message, calls an API. Your CodeIndex MCP tool layer is exactly this: agents call structured tools (get_symbol_definition, search_references) to act on the codebase rather than just generating text.

Reflection: the agent evaluates whether the action achieved the goal, and decides whether to loop or terminate. PACE's Evaluator does this — it scores whether the Coder's output satisfies the compliance policy, and if not, the loop re-runs.

**Architecture Patterns: Reactive vs Deliberative vs Hybrid**

Reactive agents respond directly to inputs without maintaining internal state or planning ahead. Fast, simple, predictable. Good for narrow tasks with well-defined input-output mappings. Bad for multi-step problems that require backtracking.

Deliberative agents build an internal model of the world, reason over it, and produce a plan before acting. Slower, more expensive, handles complex tasks. Classic example: a planner that builds a task graph before executing any step.

Hybrid agents combine both: a fast reactive layer handles routine steps, a deliberative layer kicks in for complex sub-problems. Most production agentic systems are hybrid — your PACE loop is deliberative at the planning stage (Planner reasons about which compliance checks to run) and reactive at the execution stage (Coder generates the fix given a specific instruction).

**Multi-Agent Coordination Patterns**

Orchestrator-worker: a central orchestrator agent breaks the task into subtasks and delegates to specialized worker agents. The orchestrator holds the plan; workers are stateless executors. PACE follows this loosely — Planner is the orchestrator, Coder and Author are workers. This is the most common pattern on the exam.

Pipeline (sequential): agents are chained in a fixed sequence. Output of agent N is input to agent N+1. PACE's P→A→C→E chain is a pipeline. Simple, predictable, easy to debug. Weakness: no parallelism; if step 3 fails, the whole pipeline fails.

Peer-to-peer: agents communicate directly without a central coordinator. Good for collaborative tasks but hard to reason about globally. Less common in current production systems.

Hierarchical: multi-level orchestration — a top-level orchestrator delegates to sub-orchestrators, which manage their own worker pools. Used for very large, complex workflows.

**Tool Use and Function Calling**

Tools are how agents affect the world outside the LLM. Without tools, an agent can only generate text. With tools, it can query databases, call APIs, run code, search the web, write files.

Function calling (OpenAI-style) / tool use (Anthropic-style): the LLM outputs a structured JSON object describing which tool to call and with what arguments. The host system executes the tool and returns the result. The LLM receives the result and continues reasoning.

Your CodeIndex MCP layer is a real implementation: agents call tools like get_symbol_definition(symbol="PatientRecord") and get back structured data from the SQLite knowledge graph. The tool schema (name, description, input spec) is what the LLM uses to decide when and how to call each tool. Well-written tool descriptions are critical — vague descriptions cause wrong tool selection.

**Stateless vs Stateful Agents**

Stateless agents treat each invocation independently. No memory of prior turns. Simple, horizontally scalable, but can't handle multi-turn tasks natively. You'd pass context explicitly in each call.

Stateful agents maintain state across invocations — conversation history, partial plan progress, intermediate results. Required for long-running tasks. Adds complexity around state persistence, recovery from failure, and consistency.

Your PACE pipeline is stateful within a run — each agent passes its output to the next, and the Evaluator's verdict determines whether to loop. Across runs, you'd typically externalize state to a database or message queue.

**Agent Communication Protocols**

Agents in a multi-agent system need to exchange structured information. Common patterns: shared memory (agents read/write a common store), message passing (agents send typed messages to each other), and blackboard architecture (central shared workspace all agents can read/write). The exam tends to test message passing and shared memory in the context of coordination patterns.

---

## Sub-topics Covered

- **1.1 Agent components and the perceive-plan-act-reflect cycle** — STRONG (PACE implements all four stages explicitly)
- **1.2 Architecture patterns: reactive, deliberative, hybrid** — REVIEW (know the definitions, less hands-on with reactive systems)
- **1.3 Multi-agent coordination patterns: orchestrator-worker, pipeline, peer-to-peer, hierarchical** — STRONG (PACE is a pipeline with orchestrator elements; CodeIndex uses orchestrator-worker for 16+ agents)
- **1.4 Tool use and function calling** — STRONG (CodeIndex MCP tool layer, Bedrock tool use)
- **1.5 Stateless vs stateful agents and design trade-offs** — REVIEW (understand the theory, less experience with stateless agent design patterns specifically)

---

## Key Points to Remember

- The agent loop is perceive → plan → act → reflect. Every agent architecture maps to this cycle.
- Orchestrator-worker is the dominant pattern in production multi-agent systems — know it cold.
- Tool descriptions (not just schemas) determine whether an LLM picks the right tool. Bad descriptions = wrong tool calls.
- Stateful agents require external persistence for failure recovery — in-memory state dies with the process.
- Pipeline = sequential, no parallelism. Orchestrator-worker = parallel fan-out possible.
- Hybrid architectures (reactive layer + deliberative layer) outperform pure reactive or pure deliberative in production.
- MCP (Model Context Protocol) is the standardized way to expose tools to agents — know this for NVIDIA platform questions too.

---

## Go Deeper

- `study-brain/papers/2210.03629v3.pdf` — ReAct paper. Focus on the interleaving of reasoning traces and action calls — this is the theoretical basis for the perceive-plan-act loop and will appear in cognition/planning questions too.
- `study-brain/papers/2310.10501v1.pdf` — Check for multi-agent coordination content. Look for sections on agent communication protocols and orchestration patterns.
- `study-brain/papers/2402.02716v1.pdf` — Likely covers more recent agent architecture work. Look for any content on tool use patterns and function calling schemas.

---

## Cheatsheet

**Perceive-Plan-Act-Reflect** — the four-stage loop every agent runs: receive input, reason about it, execute an action, evaluate the outcome.

**Reactive agent** — responds directly to inputs with no internal world model or planning. Fast, stateless, narrow.

**Deliberative agent** — builds an internal model and generates a plan before acting. Slower, handles complex multi-step tasks.

**Orchestrator-worker** — central agent decomposes task and delegates subtasks to specialized workers. Most common production pattern.

**Pipeline pattern** — agents chained in fixed sequence; N's output is N+1's input. Simple, predictable, no parallelism.

**Tool / Function calling** — agent outputs structured JSON (tool name + args); host executes; result returned to agent for continued reasoning.

**MCP (Model Context Protocol)** — standardized protocol for exposing tools to agents. What CodeIndex uses for its 16-agent tool layer.

**Stateless agent** — no cross-invocation memory; context passed explicitly each call. Scales horizontally, can't handle multi-turn natively.

**Stateful agent** — maintains state across invocations. Required for long-running tasks; needs external persistence for reliability.

**Hierarchical multi-agent** — top-level orchestrator delegates to sub-orchestrators, each managing their own workers. Used for large complex workflows.

---

## Exam Questions

Q1: Which component of the agent loop is responsible for deciding what action to take next based on current inputs and goals?
A) Perception
B) Memory
C) Planning
D) Reflection

Q2: An agent is designed to respond to each user message independently, with no memory of prior turns, and can be scaled across many instances. This describes a:
A) Deliberative agent
B) Stateful agent
C) Reactive agent
D) Stateless agent

Q3: In the orchestrator-worker multi-agent pattern, which agent is typically responsible for decomposing a complex task into subtasks?
A) The worker agent with the most specialized tools
B) The central orchestrator agent
C) The evaluator agent
D) The agent that completes the first subtask

Q4: What is the primary purpose of a tool schema in a function-calling agentic system?
A) To control the LLM's temperature setting during tool execution
B) To tell the LLM the name, description, and input format of each available tool
C) To authenticate API calls made by the agent
D) To log tool execution results for debugging

Q5: A team is building a compliance scanner that uses four agents: Planner, Author, Coder, and Evaluator. The Evaluator re-routes tasks back to the Coder if compliance checks fail. This architecture is best described as:
A) Peer-to-peer multi-agent with shared memory
B) Sequential pipeline with a feedback loop
C) Reactive single-agent with tool use
D) Hierarchical multi-agent with sub-orchestrators

Q6: Your RAG-based clinical assistant retrieves patient document chunks (perception), generates a summary using those chunks (planning), and writes the result to an EHR record via API (action). What is missing from a full perceive-plan-act-reflect agent loop?
A) A memory store for patient history
B) A reflection step to evaluate whether the summary meets clinical quality standards
C) A tool schema for the EHR API
D) A deliberative planning layer

Q7: An agent system needs to fan out a compliance scan across 10 policy domains simultaneously and aggregate results. Which coordination pattern is most appropriate?
A) Sequential pipeline
B) Peer-to-peer with blackboard
C) Orchestrator-worker with parallel workers
D) Single-agent with tool chaining

Q8: You're exposing a code search tool to an LLM agent. The tool schema has the correct input/output types, but in production the agent rarely calls it and prefers to answer from its training data instead. The most likely root cause is:
A) The tool's input type is too strict
B) The tool description is too vague — the LLM doesn't know when to use it
C) The LLM's context window is too small to hold the tool schema
D) Tool calling requires fine-tuning; base models don't support it

Q9: A stateful agent managing a multi-hour compliance scan crashes at step 7 of 20. When restarted, it has no record of steps 1-6 and restarts from scratch. What architectural component is missing?
A) A reflection layer to detect the failure
B) An orchestrator to re-route around the crashed agent
C) External state persistence (e.g., database or message queue checkpointing intermediate results)
D) A reactive fallback agent

Q10: Compare a pure pipeline architecture (P→A→C→E) with an orchestrator-worker architecture for the same four-step compliance task. Which statement correctly identifies a trade-off?
A) Pipeline allows parallelism; orchestrator-worker does not
B) Orchestrator-worker is simpler to debug because execution order is fixed
C) Pipeline is easier to debug (fixed execution order) but can't parallelize steps; orchestrator-worker enables parallel fan-out but adds coordination complexity
D) Orchestrator-worker eliminates the need for a planning step since workers self-organize
