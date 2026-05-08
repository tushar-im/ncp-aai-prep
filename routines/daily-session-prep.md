# Routine: Daily Session Prep

## Routine Name
NCP-AAI Daily Session Prep

## Schedule
Weekdays at 8:00 PM IST (14:30 UTC)

## Instructions (paste this as the Routine prompt)

```
You are preparing a study session for Tushar Sarang who is studying for the 
NVIDIA-Certified Professional: Agentic AI (NCP-AAI) exam on July 5, 2026.

- Prefer study-brain/ files for content already downloaded.
- Use web fetch only when study-brain/ doesn't cover the topic or when referencing NVIDIA official docs for accuracy.

Steps:
1. Get today's date in YYYY-MM-DD format
2. Read study-plan.md to find the topic scheduled for today based on current date and phase
3. Read progress.md to see what has already been covered — skip completed topics
4. Read wrong-answers.md to identify any recurring weak areas
5. Check if sessions/[today]/ already exists — if it does, skip and print "Session already prepared for today." else create the folder: sessions/[today]/
6. Write sessions/[today]/session-study.md with this exact structure:

<session-study-structure>
---
date: [YYYY-MM-DD]
topic: [topic name]
sections: [list of exam sections covered]
weight: [combined exam weight %]
phase: [current phase number]
day: [day number in 60-day plan]
days_until_exam: [days remaining to July 5, 2026]
status: pending
---

# Study Session — [date]
## Topic: [topic name]
## Exam Section: [section name] ([weight]% of exam)
## Estimated Time: 1 hour

---

## Concept Brief
[500-600 words explaining the concept at practitioner level.
Cover every numbered sub-topic from the exam blueprint for this section (e.g. 1.1, 1.2, 1.3).
Reference Tushar's actual work where possible:
- PACE = 4-agent compliance loop (Planner→Author→Coder→Evaluator)
- CodeIndex = MCP tool layer, ast-grep + SQLite
- RAG Pipeline = patient document retrieval, vector search
- AWS Bedrock = Claude clinical assistant
- de-id.org = browser-native PHI de-identification
Be specific — name patterns, frameworks, and trade-offs. Not just definitions.]

---

## Sub-topics Covered
[List each exam blueprint sub-topic number and name covered in this session.
Mark each as: STRONG (Tushar has built this) / REVIEW (knows it, needs reinforcement) / NEW (gap to close)]

---

## Key Points to Remember
[5-7 bullet points — the things most likely to appear on exam questions.
Each bullet should be a single crisp fact or distinction, not a paragraph.]

---

## Go Deeper
[2-3 specific study-brain file references with what to look for in each.
Format: `study-brain/[folder]/[file]` — [what specific section to read]]

---

## Cheatsheet
[8-10 flashcard-style entries. Format: **Term/Concept** — one crisp definition or distinction.
These should be the exact facts, names, and formulas most likely to appear on exam.
No fluff — each entry should be memorizable in under 10 seconds.]

**[Term]** — [definition]
**[Term]** — [definition]
...

---

## Exam Questions
[10 multiple choice questions. Mix of difficulty:
- Q1-4: foundational concept recall
- Q5-7: applied understanding (given a scenario, pick the right pattern/tool)
- Q8-10: harder application (diagnose a failure, compare trade-offs, pick the best design)
Each question has 4 options (A/B/C/D). No trick questions — exam-realistic only.]

Q1: ...
A) ...
B) ...
C) ...
D) ...

[repeat for Q2 through Q10]

</session-study-structure>

7. Create sessions/[today]/answers.md with this exact structure:

<answers-structure>
---
date: [YYYY-MM-DD]
topic: [topic name]
score: pending
---

# Answers — [date]
## Topic: [topic]

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
Q1: [letter] — [one line explanation]
Q2: [letter] — [one line explanation]
Q3: [letter] — [one line explanation]
Q4: [letter] — [one line explanation]
Q5: [letter] — [one line explanation]
Q6: [letter] — [one line explanation]
Q7: [letter] — [one line explanation]
Q8: [letter] — [one line explanation]
Q9: [letter] — [one line explanation]
Q10: [letter] — [one line explanation]
</answers-structure>

8. Do not run the actual study session. Just prepare the files so Tushar can run /start-study when he sits down.

9. Print a one-line summary: "Session ready: [topic] — sessions/[date]/ — [days_until_exam] days to exam"
```

## Setup Steps
1. Go to claude.ai/code/routines
2. Click New Routine
3. Name: "NCP-AAI Daily Session Prep"
4. Paste the instructions above into the prompt field
5. Select repository: your ncp-aai-prep GitHub repo
6. Trigger: Schedule → Weekdays → 7:00 AM your timezone
7. Click Create
