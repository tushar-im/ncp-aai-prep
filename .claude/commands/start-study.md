# Start Study

## CRITICAL RULES — NEVER VIOLATE THESE
1. NEVER create or modify session-study.md — it is owned by the Routine, not by you
2. NEVER generate your own questions — use ONLY the questions from session-study.md
3. NEVER show all questions at once — ask ONE question at a time
4. NEVER suggest or complete answers — wait silently for Tushar's response
5. NEVER start a new session if session-study.md exists — read it and follow it exactly
6. NEVER skip to a different topic — follow session-study.md exactly as written

## Steps
1. Get today's date in YYYY-MM-DD format
2. Check if sessions/[today]/session-study.md exists
   - If YES: read it fully. This is your script. Follow it exactly.
   - If NO: tell Tushar "No session prepared for today. The Routine may not have run yet. You can run /coach [topic] to study manually."
   - NEVER create session-study.md yourself under any circumstance.
3. Use ctx_search to pull relevant chunks from study-brain/ matching today's topic
4. Begin immediately — state topic and exam section weight, then deliver the Concept Brief from session-study.md verbatim

## Teaching Phase
- Read the Concept Brief from session-study.md and present it
- Walk through the Cheatsheet items — say "here are the key terms for today"
- Ask "Ready for questions?"

## Question Phase — STRICT RULES
- Read ALL 10 questions from session-study.md
- Ask Q1 first. Wait for answer. Do not proceed until Tushar responds.
- After each answer: say correct/incorrect, one line explanation, then ask next question
- Do NOT show answer options for the next question until the current one is answered
- Do NOT use autocomplete-friendly phrasing — never end a question with the answer word
- Keep a running score mentally: X/10

## After All 10 Questions
- State final score: X/10
- Read Correct Answers section from answers.md
- For each wrong answer: explain why in 2-3 sentences
- Ask Tushar to fill in answers.md score field

## After Session Ends
When Tushar says "done", "end session", or similar:
1. Write sessions/[today]/session-notes.md
2. Move today's topic from Pending to Completed in progress.md with date and score
3. Append wrong answers to wrong-answers.md
4. Append vocab gaps to vocab-gaps.md (create if doesn't exist)
5. Tell Tushar tomorrow's topic from study-plan.md
