---
date: 2026-05-11
topic: Agent Development
sections:
  - Agent Development
weight: 15%
phase: 1
day: 4
days_until_exam: 55
status: pending
---

# Study Session — 2026-05-11
## Topic: Agent Development
## Exam Section: Agent Development (15% of exam)
## Estimated Time: 1 hour

---

## Concept Brief

Agent Development is where architecture becomes code. This section covers the mechanics of building agents that actually work in production — tool use, error handling, multi-agent communication, streaming, state management, testing.

**Tool Use and Function Calling**

Agents extend beyond text by calling tools. You define each tool as a JSON Schema — name, description, parameters, required fields. The model decides when to call based on context, and the description does most of the work. A vague description means the model either skips the tool when it should call it, or calls it when it shouldn't. When the model selects a tool, you execute it externally, then inject the result back as a tool-role message so the model can continue. That cycle — model picks tool, you execute, result injected — is the core loop.

Parallel tool calls let the model request multiple tools in a single response, running them concurrently. Your CodeIndex MCP layer does exactly this: when an agent asks about a codebase, you might call `find_symbol`, `get_dependencies`, and `list_callers` simultaneously rather than sequentially. That's the difference between 600ms and 200ms per query across 16 agents.

**Error Handling and Resilience**

Agents touch external systems — APIs, databases, other models. Three failure categories matter on the exam: tool execution errors (bad input, service down, timeout), model errors (context length exceeded, content policy hit), and orchestration errors (routing failure, state corruption). Each needs a different recovery path.

Retry with exponential backoff handles transient failures and rate limits. Circuit breakers stop retrying a consistently-failing dependency to prevent cascade failures — after N failures in a window, fast-fail and return an error rather than hammering a downed service. PACE's Evaluator feedback loop is application-level error recovery: Evaluator scores below threshold → structured re-route message to orchestrator → orchestrator sends artifact back to Coder with specific failure notes. That's different from infrastructure-level retry — it's semantic error recovery.

**Multi-Agent Communication**

Agents communicate via message passing. Key primitives: handoffs (pass context and control to one specific agent), broadcasts (send to all), and structured replies (return result to orchestrator). Handoff quality determines what the receiving agent can do — you need to package the right context. In PACE, Coder→Evaluator passes the compliance artifact and the original task spec. The Evaluator does not need the Planner's reasoning or the Author's draft — sending those causes context bleed and anchoring bias (that's what bit you in the May 8 session). Scope handoff context deliberately.

Do not conflate message passing with blackboard architecture. Blackboard = shared passive data store that all agents read from and write to, with no direct agent-to-agent messaging. PACE is a sequential pipeline with a feedback loop, not a blackboard.

**Streaming and Async**

Streaming delivers tokens as they're generated rather than buffering the full response. In Python: `async for chunk in stream:`. The implementation concern most likely to appear on the exam is backpressure — when a downstream consumer processes tokens slower than the model produces them, you need a queue or flow control mechanism. Without it, you either block the producer or drop tokens. Your Bedrock clinical assistant uses streaming for real-time patient summaries — the responsiveness is the UX, but the backpressure handling is what makes it not crash under load.

Async tool calls execute concurrently without blocking. Pattern: fire multiple tool coroutines with `asyncio.gather()`, await all results, inject into context.

**State Management**

Two levels: conversation-level state (the message history, in-memory, lives one session) and persistent state (database-backed, survives restarts). Checkpointing saves state at decision points so a failed multi-step workflow can resume from the last checkpoint rather than restart from zero. LangGraph compiles this natively via graph checkpointers. State serialization must be deterministic — same serialized state must reconstruct to the same execution path.

State machines add explicit structure: enumerate valid states, define valid transitions, and catch attempts to make illegal transitions. Useful for compliance-critical workflows like PACE where you need to guarantee Planner always runs before Author.

**Testing and Debugging**

Unit test tools in isolation with mocked inputs. Integration test the full agent flow with fixed golden inputs and expected outputs. Trace-based debugging (LangSmith, NVIDIA AgentIQ) captures every model call, tool invocation, input/output pair, and timing — you can replay a run and see exactly where it diverged. Prompt regression testing: run a golden dataset before and after a prompt change and compare scores. Evaluation harnesses parallelize this across large test suites and use judge models to score — much faster than manual review and scalable to CI pipelines.

---

## Sub-topics Covered

- **2.1 Tool use and function calling** — STRONG (CodeIndex MCP layer, Bedrock tool use)
- **2.2 Error handling and resilience in agent workflows** — STRONG (PACE feedback loop, retry patterns)
- **2.3 Multi-agent communication and handoff patterns** — STRONG (PACE architecture)
- **2.4 Streaming and async agent capabilities** — STRONG (Bedrock clinical assistant)
- **2.5 Agent state and context management** — REVIEW (know the concepts, less hands-on with checkpointing)
- **2.6 Testing and debugging agent systems** — REVIEW (have done manual testing, less formal harness work)
- **2.7 Multimodal capability integration** — NEW (not directly built — needs reinforcement)

---

## Key Points to Remember

- Tool descriptions are load-bearing — vague description = wrong invocation frequency, not just wrong output.
- Parallel tool calls are a model capability, not a client-side pattern — the model requests multiple tools in a single response turn.
- Context bleed in handoffs causes anchoring bias in evaluators — pass only what the receiving agent needs, not the full reasoning chain.
- Circuit breaker vs retry: retry handles transient failures; circuit breaker handles sustained failures by fast-failing after a threshold.
- Checkpoint-based state enables resumption, not just logging — the distinction matters for exam scenarios about long-running workflows.
- Backpressure is the streaming failure mode — producer outpacing consumer requires a queue or flow control, not a faster consumer.
- Blackboard architecture = shared passive store + agents that read/write it — distinct from pipeline + feedback loop (PACE is the latter).

---

## Go Deeper

- `study-brain/ragas/agents.md` — review how RAGAs conceptualizes agent evaluation flows; relevant to the test/debug sub-topic
- `study-brain/papers/2210.03629v3.pdf` — likely ReAct paper; check the tool-use framing and how it handles tool errors and retries
- `study-brain/papers/2402.02716v1.pdf` — check if this covers agent communication patterns or state management; read the abstract and intro section

---

## Cheatsheet

**Function calling** — model selects a named tool from a provided JSON schema and returns structured arguments; caller executes and injects result as a tool message

**Tool schema** — JSON Schema defining tool name, description, and parameters; description quality directly determines when the model invokes it

**Parallel tool calls** — model requests multiple tools in a single response; client executes concurrently and returns all results before next model turn

**Handoff** — structured transfer of context and control from one agent to a specific other agent; context should be scoped to what the receiver needs

**Backpressure** — flow control mechanism where a consumer signals a producer to slow down when the consumer can't keep pace; required in streaming pipelines

**Circuit breaker** — stops retrying a failing dependency after N failures in a time window; fast-fails subsequent calls to prevent cascade failure

**Exponential backoff** — retry delay doubles with each attempt (e.g. 1s → 2s → 4s → 8s); handles rate limits and transient failures

**Checkpoint** — serialized state snapshot at a workflow decision point; enables resumption after failure without restarting from scratch

**Trace** — full record of one agent execution: model inputs/outputs, tool calls, results, timing; used for debugging and regression testing

**Blackboard architecture** — agents share a passive data store they all read/write; no direct agent-to-agent messaging (contrast with PACE's pipeline + feedback loop)

---

## Exam Questions

Q1: What is the primary role of the tool description field in a function-calling schema?
A) It sets the maximum number of times the tool can be called per turn
B) It tells the model when and how to invoke the tool, directly shaping invocation decisions
C) It validates the return type of the tool result
D) It specifies the authentication method for the external service

Q2: Which component must be injected back into the model context after a tool executes successfully?
A) An updated system prompt containing the result
B) A tool-role message with the tool name and its return value
C) A new user message summarizing what the tool did
D) An assistant message with the tool result appended

Q3: An agent retries a failing API call 10 times with a fixed 1-second delay and still fails. What pattern would better handle a dependency that is rate-limiting the agent?
A) Increase retries to 50 with the same fixed delay
B) Switch to a different API endpoint on each retry
C) Retry with exponential backoff and jitter to spread load
D) Cache the last successful result and return it indefinitely

Q4: In a multi-agent pipeline, what distinguishes a "handoff" from a "broadcast"?
A) A handoff uses shared memory; a broadcast uses message queues
B) A handoff terminates the sending agent; a broadcast does not
C) A handoff passes context and control to one specific agent; a broadcast sends to all agents
D) A handoff is synchronous; a broadcast is asynchronous

Q5: An orchestrator sends the full reasoning chains of the Planner, Author, and Coder agents to the Evaluator as context. The Evaluator consistently rates outputs higher than they deserve. What is the most likely cause?
A) The Evaluator model is too small to process large contexts accurately
B) Anchoring bias from context bleed — the Evaluator is influenced by prior agents' justifications
C) The scoring rubric is too permissive
D) The Planner's reasoning is overriding the Evaluator's scoring logic

Q6: A streaming agent pipeline delivers tokens to a UI. During peak load, the UI reports dropped tokens and connection resets. The model is producing tokens faster than the UI can render them. What is the correct fix?
A) Increase the model's chunk size to send fewer, larger payloads
B) Implement backpressure handling with an async queue between the model stream and the UI
C) Reduce temperature to make the model produce tokens more slowly
D) Switch from streaming to batch responses during peak hours

Q7: You need to add a tool to a medical agent that queries patient lab results. The tool should only be called for lab-related questions, not general clinical questions. What is the most effective single change to control this behavior?
A) Add a post-processing filter that checks if the tool was called correctly
B) Reduce the number of tools available to force the model to use this one
C) Write a precise, scoped tool description that clearly specifies the trigger conditions
D) Add a pre-prompt reminding the model not to call tools unnecessarily

Q8: A multi-step compliance agent processes a document through 6 sequential stages. The pipeline fails at stage 5 after 45 minutes of work. The team wants to rerun from stage 5 without redoing stages 1-4. What must be implemented?
A) Idempotent tool calls so stages 1-4 can be safely rerun in seconds
B) A checkpoint at each stage that serializes the workflow state to a durable store
C) Parallel execution of all stages to reduce total time
D) A fallback model that completes stage 5 using the original input

Q9: You are debugging an agent that intermittently fails to call the correct tool. Tracing reveals the tool schemas are consistent, inputs are identical, but tool selection varies across runs. What is the most likely root cause?
A) The tracing system is not capturing all tool calls accurately
B) A non-zero temperature setting causes sampling variance in the model's tool selection
C) The tool execution environment has a race condition
D) The context is being truncated inconsistently by the model

Q10: A team needs to validate that a code-generation agent produces correct outputs across 200 regression test cases after each prompt update. Manual review takes 3 hours per run, blocking deployments. What is the best architectural solution?
A) Run a subset of 20 cases manually and extrapolate coverage
B) Deploy to staging and observe production traffic with LangSmith traces
C) Build an evaluation harness that runs all 200 cases in parallel and scores outputs with a judge model, integrated into CI
D) Have the agent self-evaluate its outputs using chain-of-thought scoring
