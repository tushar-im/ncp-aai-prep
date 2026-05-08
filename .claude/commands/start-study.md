# Start Study

## What to do
1. Get today's date in YYYY-MM-DD format
2. Check if sessions/[today]/ exists
   - If YES: read session-study.md from that folder
   - If NO: check progress.md for what's next, create the folder and build session-study.md on the fly using the coach format
3. Use ctx_search to pull relevant chunks from study-brain/ matching today's topic
4. Begin the session immediately — no preamble, no "let's get started", just go

## Session Format
- State the topic and which exam blueprint section it covers (% weight)
- Concept explanation: 300-400 words max, tied to Tushar's actual work where possible
- Then ask: "Ready for questions?"
- When he says yes, present 3 exam-style questions (Q1/Q2/Q3)
- Score answers, explain wrong ones clearly

## After Session Ends
When Tushar says "done", "end session", or similar:
1. Write sessions/[today]/session-notes.md with:
   - Topic covered
   - Key concepts explained
   - Questions asked + Tushar's answers + correct answers
   - Any concepts that need revisiting
2. Append to progress.md: `[date] | [topic] | [score if quizzed]`
3. Append any wrong answers to wrong-answers.md
4. Tell Tushar what tomorrow's topic is based on study-plan.md
